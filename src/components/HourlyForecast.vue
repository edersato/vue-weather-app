<template>
  <div v-if="hourlyForecastAvailable" class="hourly-section glass">
    <div class="section-header">
      <h3 class="section-title">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
          <circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/>
        </svg>
        Previsão 24h
      </h3>
      <span class="hour-count">{{ filteredHours.length }} horas</span>
    </div>

    <!-- Temperature sparkline -->
    <div class="sparkline-wrap" v-if="tempRange > 0">
      <svg :viewBox="`0 0 ${svgW} ${svgH}`" class="sparkline-svg" preserveAspectRatio="none">
        <defs>
          <linearGradient id="sparkGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#4fa3e3" stop-opacity="0.35"/>
            <stop offset="100%" stop-color="#4fa3e3" stop-opacity="0"/>
          </linearGradient>
          <linearGradient id="lineGrad" x1="0" y1="0" x2="1" y2="0">
            <stop offset="0%" stop-color="#4fa3e3"/>
            <stop offset="100%" stop-color="#7dd3fc"/>
          </linearGradient>
        </defs>
        <!-- Fill area -->
        <path :d="areaPath" fill="url(#sparkGrad)"/>
        <!-- Line -->
        <path :d="linePath" fill="none" stroke="url(#lineGrad)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        <!-- Dots at extremes -->
        <circle v-for="pt in extremeDots" :key="pt.label"
          :cx="pt.x" :cy="pt.y" r="3" fill="#4fa3e3"/>
      </svg>
    </div>

    <!-- Horizontal scroll cards -->
    <div class="hourly-scroll">
      <div
        v-for="(hour, index) in filteredHours"
        :key="index"
        class="hour-card"
        :class="{ 'is-now': isCurrentHour(hour.time) }"
      >
        <span class="h-time">{{ formatTime(hour.time) }}</span>
        <img :src="'https:' + hour.condition.icon" :alt="hour.condition.text" class="h-icon" />
        <span class="h-temp">{{ Math.round(hour.temp_c) }}°</span>
        <div class="h-rain" :style="{ opacity: hour.chance_of_rain > 0 ? 1 : 0.3 }">
          <svg width="10" height="10" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 2C6 10 4 14 4 17a8 8 0 0 0 16 0c0-3-2-7-8-15z"/>
          </svg>
          <span>{{ hour.chance_of_rain }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["forecast"],
  data() { return { svgW: 800, svgH: 60 }; },
  computed: {
    filteredHours() {
      const today = new Date().toISOString().split('T')[0];
      const tomorrow = new Date(Date.now() + 86400000).toISOString().split('T')[0];
      const days = this.forecast?.forecast?.forecastday || [];
      const todayHours = days.find(d => d.date === today)?.hour || [];
      const tomorrowHours = days.find(d => d.date === tomorrow)?.hour || [];
      const allHours = [...todayHours, ...tomorrowHours];
      const nowH = new Date().getHours();
      // Return 24 hours starting from current hour
      const startIdx = todayHours.findIndex((_, i) => i >= nowH);
      if (startIdx === -1) return allHours.slice(0, 24);
      return allHours.slice(startIdx, startIdx + 24);
    },
    hourlyForecastAvailable() { return this.filteredHours.length > 0; },
    temps() { return this.filteredHours.map(h => h.temp_c); },
    tempMin() { return Math.min(...this.temps); },
    tempMax() { return Math.max(...this.temps); },
    tempRange() { return this.tempMax - this.tempMin; },
    linePath() {
      if (!this.temps.length || this.tempRange === 0) return '';
      const pad = 6;
      const pts = this.temps.map((t, i) => {
        const x = (i / (this.temps.length - 1)) * this.svgW;
        const y = pad + (1 - (t - this.tempMin) / this.tempRange) * (this.svgH - pad * 2);
        return `${x},${y}`;
      });
      return 'M' + pts.join('L');
    },
    areaPath() {
      if (!this.linePath) return '';
      return `${this.linePath}L${this.svgW},${this.svgH}L0,${this.svgH}Z`;
    },
    extremeDots() {
      if (!this.temps.length) return [];
      const maxI = this.temps.indexOf(this.tempMax);
      const minI = this.temps.indexOf(this.tempMin);
      const pad = 6;
      const pt = (i) => ({
        x: (i / (this.temps.length - 1)) * this.svgW,
        y: pad + (1 - (this.temps[i] - this.tempMin) / this.tempRange) * (this.svgH - pad * 2),
      });
      return [
        { label: 'max', ...pt(maxI) },
        { label: 'min', ...pt(minI) },
      ];
    },
  },
  methods: {
    formatTime(time) {
      return new Date(time).toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' });
    },
    isCurrentHour(time) {
      const now = new Date();
      const t = new Date(time);
      return now.getHours() === t.getHours() && now.toDateString() === t.toDateString();
    },
  },
};
</script>

<style lang="scss" scoped>
.hourly-section {
  padding: 1.4rem 1.5rem 1.25rem;
}

.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.section-title {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  font-family: var(--font-display);
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  svg { opacity: 0.6; }
}

.hour-count {
  font-size: 0.72rem;
  color: var(--text-muted);
  letter-spacing: 0.03em;
}

/* Sparkline */
.sparkline-wrap {
  margin: 0 -0.25rem 0.75rem;
  height: 60px;
  overflow: hidden;
  border-radius: 8px;
}

.sparkline-svg {
  width: 100%;
  height: 100%;
}

/* Scroll track */
.hourly-scroll {
  display: flex;
  gap: 0.5rem;
  overflow-x: auto;
  padding-bottom: 0.4rem;
  scrollbar-width: thin;
  scrollbar-color: rgba(255,255,255,0.1) transparent;
  scroll-snap-type: x mandatory;
}

/* Individual hour card */
.hour-card {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  padding: 0.65rem 0.6rem;
  min-width: 64px;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.06);
  border-radius: 14px;
  scroll-snap-align: start;
  transition: background var(--transition), border-color var(--transition);

  &:hover { background: var(--glass-hover); }

  &.is-now {
    background: rgba(79,163,227,0.12);
    border-color: rgba(79,163,227,0.3);
    .h-time { color: var(--accent-cold); font-weight: 600; }
  }
}

.h-time {
  font-size: 0.72rem;
  font-weight: 500;
  color: var(--text-muted);
  letter-spacing: 0.02em;
}

.h-icon {
  width: 32px; height: 32px;
  object-fit: contain;
}

.h-temp {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--text-primary);
}

.h-rain {
  display: flex;
  align-items: center;
  gap: 0.2rem;
  font-size: 0.68rem;
  color: #7dd3fc;
  span { font-weight: 500; }
  svg { flex-shrink: 0; }
}

@media (max-width: 480px) {
  .hourly-section { padding: 1.1rem 1rem 1rem; }
  .hour-card { min-width: 56px; padding: 0.55rem 0.45rem; }
}
</style>