<template>
  <div v-if="weatherDefine" class="city-info-root">

    <!-- Hero card -->
    <div class="hero-card glass" :class="tempClass">
      <div class="hero-top">
        <div class="location-block">
          <div class="city-name">{{ weather.name }}<span class="country">, {{ weather.sys.country }}</span></div>
          <div class="description">{{ weather.weather[0].description }}</div>
        </div>
        <div class="weather-icon-wrap">
          <WeatherIcon :code="weather.weather[0].id" :size="iconSize" />
        </div>
      </div>

      <div class="hero-center">
        <div class="temp-main">{{ Math.round(weather.main.temp) }}<span class="deg">°C</span></div>
        <div class="feels-like">Sensação {{ Math.round(weather.main.feels_like) }}°C</div>
      </div>

      <div class="hero-stats">
        <div class="stat-chip">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path d="M12 2v10M12 22a5 5 0 1 0 0-10 5 5 0 0 0 0 10z"/>
          </svg>
          <span>{{ Math.round(weather.main.temp_min) }}°  /  {{ Math.round(weather.main.temp_max) }}°</span>
          <small>Mín / Máx</small>
        </div>
        <div class="stat-chip">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path d="M12 2a7 7 0 0 0-7 7c0 5 7 13 7 13s7-8 7-13a7 7 0 0 0-7-7z"/>
          </svg>
          <span>{{ weather.main.humidity }}%</span>
          <small>Umidade</small>
        </div>
        <div class="stat-chip">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path d="M17.7 7.7a2.5 2.5 0 1 1 1.8 4.3H2"/><path d="M9.6 4.6A2 2 0 1 1 11 8H2"/><path d="M12.6 19.4A2 2 0 1 0 14 16H2"/>
          </svg>
          <span>{{ weather.wind.speed }} m/s</span>
          <small>Vento</small>
        </div>
        <div class="stat-chip" v-if="weather.visibility">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
            <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7z"/><circle cx="12" cy="12" r="3"/>
          </svg>
          <span>{{ (weather.visibility / 1000).toFixed(1) }} km</span>
          <small>Visibilidade</small>
        </div>
      </div>
    </div>

    <!-- Subcomponents -->
    <HourlyForecast :forecast="forecast" />
    <Forecast :forecast="forecast" />
  </div>
</template>

<script>
import HourlyForecast from './HourlyForecast.vue';
import Forecast from './Forecast.vue';
import WeatherIcon from './WeatherIcon.vue';

export default {
  props: ["weather", "forecast"],
  components: { Forecast, HourlyForecast, WeatherIcon },
  computed: {
    weatherAvailable() {
      return typeof this.weather.main !== "undefined";
    },
    weatherDefine() {
      return this.weatherAvailable && this.forecast;
    },
    tempClass() {
      if (!this.weatherAvailable) return "";
      return this.weather.main.temp > 25 ? "hot-tint" : "cold-tint";
    },
    iconSize() {
      return typeof window !== 'undefined' && window.innerWidth <= 520 ? 64 : 88;
    },
  },
};
</script>

<style lang="scss" scoped>
.city-info-root {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  animation: fadeUp 0.5s ease both;
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* Hero card */
.hero-card {
  padding: 1.75rem 2rem 1.5rem;
  position: relative;
  overflow: hidden;
  transition: border-color 0.6s ease;

  &::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: inherit;
    opacity: 0;
    transition: opacity 0.6s ease;
    pointer-events: none;
  }

  &.cold-tint {
    border-color: rgba(79,163,227,0.2);
    &::before {
      background: radial-gradient(ellipse at top left, rgba(79,163,227,0.07), transparent 60%);
      opacity: 1;
    }
  }
  &.hot-tint {
    border-color: rgba(249,127,59,0.2);
    &::before {
      background: radial-gradient(ellipse at top left, rgba(249,127,59,0.07), transparent 60%);
      opacity: 1;
    }
  }
}

/* Hero top row */
.hero-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1.25rem;
}

.city-name {
  font-family: var(--font-display);
  font-size: 1.65rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  line-height: 1.1;
  .country { font-weight: 400; color: var(--text-secondary); }
}

.description {
  margin-top: 0.3rem;
  font-size: 0.85rem;
  color: var(--text-secondary);
  text-transform: capitalize;
  letter-spacing: 0.03em;
}

.weather-icon-wrap {
  flex-shrink: 0;
  margin-left: 1rem;
}

.weather-icon {
  width: 72px; height: 72px;
  object-fit: contain;
  filter: drop-shadow(0 0 18px rgba(79,163,227,0.35));
}

/* Temperature */
.hero-center {
  display: flex;
  align-items: baseline;
  gap: 0.75rem;
  margin-bottom: 1.75rem;
}

.temp-main {
  font-family: var(--font-display);
  font-size: 5.5rem;
  font-weight: 700;
  letter-spacing: -0.05em;
  line-height: 1;
  color: var(--text-primary);

  .deg {
    font-size: 2.5rem;
    font-weight: 300;
    color: var(--text-secondary);
  }
}

.feels-like {
  font-size: 0.82rem;
  color: var(--text-muted);
  margin-bottom: 0.35rem;
  align-self: flex-end;
}

/* Stats row */
.hero-stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.65rem;
}

.stat-chip {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.2rem;
  padding: 0.7rem 0.5rem;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.06);
  border-radius: 12px;
  text-align: center;

  svg { color: var(--text-muted); margin-bottom: 0.1rem; }
  span { font-size: 0.9rem; font-weight: 500; color: var(--text-primary); }
  small { font-size: 0.68rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.06em; }
}

/* Mobile */
@media (max-width: 520px) {
  .hero-card { padding: 1.35rem 1.25rem 1.25rem; }
  .city-name { font-size: 1.3rem; }
  .temp-main { font-size: 4rem; .deg { font-size: 1.8rem; } }
  .hero-stats { grid-template-columns: repeat(2, 1fr); }
  .weather-icon { width: 54px; height: 54px; }
}
</style>