<template>
  <div v-if="forecastAvailable" class="forecast-section glass">
    <div class="section-header">
      <h3 class="section-title">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
          <rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/>
        </svg>
        Próximos Dias
      </h3>
    </div>

    <div class="days-grid">
      <div
        v-for="(day, index) in forecastDays"
        :key="index"
        class="day-card"
        :class="{ 'is-today': index === 0 }"
      >
        <div class="day-header">
          <span class="day-label">{{ getDayLabel(day.date, index) }}</span>
          <span class="day-date">{{ formatShortDate(day.date) }}</span>
        </div>

        <div class="day-body">
          <img :src="'https:' + day.day.condition.icon" :alt="day.day.condition.text" class="day-icon" />
          <div class="day-condition">{{ day.day.condition.text }}</div>
        </div>

        <div class="day-temps">
          <div class="temp-bar-wrap">
            <span class="t-min">{{ Math.round(day.day.mintemp_c) }}°</span>
            <div class="temp-bar">
              <div
                class="temp-fill"
                :style="{ width: tempBarWidth(day.day.mintemp_c, day.day.maxtemp_c) + '%' }"
              ></div>
            </div>
            <span class="t-max">{{ Math.round(day.day.maxtemp_c) }}°</span>
          </div>
        </div>

        <div class="day-extras">
          <div class="extra-chip" :style="{ opacity: day.day.daily_chance_of_rain > 0 ? 1 : 0.4 }">
            <svg width="11" height="11" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 2C6 10 4 14 4 17a8 8 0 0 0 16 0c0-3-2-7-8-15z"/>
            </svg>
            <span>{{ day.day.daily_chance_of_rain }}%</span>
          </div>
          <div class="extra-chip">
            <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M12 2a7 7 0 0 0-7 7c0 5 7 13 7 13s7-8 7-13a7 7 0 0 0-7-7z"/>
            </svg>
            <span>{{ day.day.avghumidity }}%</span>
          </div>
          <div class="extra-chip">
            <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M17.7 7.7a2.5 2.5 0 1 1 1.8 4.3H2"/>
            </svg>
            <span>{{ Math.round(day.day.maxwind_kph) }} km/h</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["forecast"],
  computed: {
    forecastDays() {
      const days = this.forecast?.forecast?.forecastday || [];
      return days.slice(0, 4); // today + 3
    },
    forecastAvailable() {
      return this.forecastDays.length > 0;
    },
    globalMinTemp() {
      return Math.min(...this.forecastDays.map(d => d.day.mintemp_c));
    },
    globalMaxTemp() {
      return Math.max(...this.forecastDays.map(d => d.day.maxtemp_c));
    },
  },
  methods: {
    getDayLabel(dateString, index) {
      if (index === 0) return "Hoje";
      if (index === 1) return "Amanhã";
      return new Date(dateString + 'T12:00:00').toLocaleDateString("pt-BR", { weekday: "long" });
    },
    formatShortDate(dateString) {
      return new Date(dateString + 'T12:00:00').toLocaleDateString("pt-BR", {
        day: "numeric", month: "short"
      });
    },
    tempBarWidth(min, max) {
      const range = this.globalMaxTemp - this.globalMinTemp;
      if (range === 0) return 60;
      const left = ((min - this.globalMinTemp) / range) * 100;
      const width = ((max - min) / range) * 100;
      return Math.max(width, 15);
    },
  },
};
</script>

<style lang="scss" scoped>
.forecast-section {
  padding: 1.4rem 1.5rem 1.5rem;
}

.section-header {
  margin-bottom: 1.1rem;
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

/* 3-day grid */
.days-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.65rem;
}

.day-card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.06);
  border-radius: 16px;
  padding: 1rem 0.85rem;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  transition: background var(--transition), border-color var(--transition), transform var(--transition);

  &:hover {
    background: var(--glass-hover);
    transform: translateY(-2px);
  }

  &.is-today {
    background: rgba(79,163,227,0.08);
    border-color: rgba(79,163,227,0.22);
    .day-label { color: var(--accent-cold); }
  }
}

.day-header {
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
}

.day-label {
  font-family: var(--font-display);
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--text-primary);
  text-transform: capitalize;
  letter-spacing: -0.01em;
}

.day-date {
  font-size: 0.7rem;
  color: var(--text-muted);
  text-transform: capitalize;
}

.day-body {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.35rem;
}

.day-icon {
  width: 44px; height: 44px;
  object-fit: contain;
  filter: drop-shadow(0 0 8px rgba(79,163,227,0.2));
}

.day-condition {
  font-size: 0.72rem;
  color: var(--text-muted);
  text-align: center;
  line-height: 1.3;
}

/* Temperature range bar */
.day-temps { margin-top: auto; }

.temp-bar-wrap {
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.t-min, .t-max {
  font-size: 0.78rem;
  font-weight: 600;
  white-space: nowrap;
}
.t-min { color: var(--text-muted); }
.t-max { color: var(--text-primary); }

.temp-bar {
  flex: 1;
  height: 4px;
  background: rgba(255,255,255,0.08);
  border-radius: 4px;
  overflow: hidden;
}

.temp-fill {
  height: 100%;
  background: linear-gradient(90deg, #4fa3e3, #7dd3fc);
  border-radius: 4px;
  min-width: 20%;
}

/* Extras row */
.day-extras {
  display: flex;
  gap: 0.3rem;
  flex-wrap: wrap;
}

.extra-chip {
  display: flex;
  align-items: center;
  gap: 0.2rem;
  font-size: 0.68rem;
  color: #7dd3fc;
  background: rgba(79,163,227,0.08);
  border-radius: 6px;
  padding: 0.2rem 0.4rem;
  span { font-weight: 500; color: var(--text-secondary); }
}

/* Responsive */
@media (max-width: 620px) {
  .days-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 380px) {
  .days-grid { grid-template-columns: 1fr; }
  .forecast-section { padding: 1.1rem 1rem; }
}
</style>