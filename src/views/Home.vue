<template>
  <div id="home" :class="['home-wrapper', weatherClass]">
    <!-- Atmospheric background layers -->
    <div class="atmo-layer atmo-1"></div>
    <div class="atmo-layer atmo-2"></div>
    <div class="grid-overlay"></div>

    <div class="app-shell">
      <header class="app-header">
        <div class="logo">
          <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M12 2a7 7 0 0 1 7 7c0 5-7 13-7 13S5 14 5 9a7 7 0 0 1 7-7z"/>
            <circle cx="12" cy="9" r="2.5"/>
          </svg>
          <span>Atmosfera</span>
        </div>
        <div class="date-badge">{{ currentDate }}</div>
      </header>

      <SearchBar @weather-fetched="setWeather" @forecast-fetched="setForecast" />
      <CityInfo :weather="weather" :forecast="forecast" />

      <footer class="app-footer" v-if="hasWeather">
        <span>Dados via OpenWeather &amp; WeatherAPI</span>
      </footer>
    </div>
  </div>
</template>

<script>
import CityInfo from "../components/CityInfo.vue";
import SearchBar from "../components/SearchBar.vue";

export default {
  components: { SearchBar, CityInfo },
  data() {
    return {
      weather: {},
      forecast: {},
    };
  },
  computed: {
    hasWeather() {
      return typeof this.weather.main !== "undefined";
    },
    weatherClass() {
      if (!this.hasWeather) return "state-default";
      return this.weather.main.temp > 25 ? "state-hot" : "state-cold";
    },
    currentDate() {
      return new Date().toLocaleDateString("pt-BR", {
        weekday: "long", day: "numeric", month: "long"
      });
    },
  },
  methods: {
    setWeather(data) { this.weather = data; },
    setForecast(data) { this.forecast = data; },
  },
};
</script>

<style lang="scss" scoped>
.home-wrapper {
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

/* Atmospheric blobs */
.atmo-layer {
  position: fixed;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.18;
  pointer-events: none;
  transition: background 1.2s ease, opacity 1.2s ease;
  z-index: 0;
}

.atmo-1 {
  width: 600px; height: 600px;
  top: -200px; left: -150px;
  background: #2a5f9e;
}
.atmo-2 {
  width: 500px; height: 500px;
  bottom: -100px; right: -100px;
  background: #1a3a6a;
}

.state-hot .atmo-1 { background: #c0521a; opacity: 0.22; }
.state-hot .atmo-2 { background: #9b3a10; opacity: 0.22; }
.state-cold .atmo-1 { background: #1a5a8a; }
.state-cold .atmo-2 { background: #0d3d6b; }

/* Subtle grid */
.grid-overlay {
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(255,255,255,0.02) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px);
  background-size: 48px 48px;
  pointer-events: none;
  z-index: 0;
}

.app-shell {
  position: relative;
  z-index: 1;
  max-width: 860px;
  margin: 0 auto;
  padding: 0 1.25rem 3rem;
}

/* Header */
.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 0 0.5rem;
  margin-bottom: 0.5rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-family: var(--font-display);
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  color: var(--text-primary);
  opacity: 0.9;

  svg { opacity: 0.7; }
}

.date-badge {
  font-size: 0.78rem;
  font-weight: 400;
  color: var(--text-secondary);
  text-transform: capitalize;
  letter-spacing: 0.02em;
}

.app-footer {
  text-align: center;
  margin-top: 2.5rem;
  font-size: 0.72rem;
  color: var(--text-muted);
  letter-spacing: 0.04em;
}

/* Mobile */
@media (max-width: 480px) {
  .app-header { padding: 1.2rem 0 0.25rem; }
  .date-badge { display: none; }
}
</style>