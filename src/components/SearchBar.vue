<template>
  <div class="search-wrapper">
    <div class="search-field" :class="{ focused, loading }">
      <svg class="search-icon" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
        <circle cx="11" cy="11" r="7"/><path d="m21 21-4.35-4.35"/>
      </svg>
      <input
        ref="input"
        type="text"
        placeholder="Busque uma cidade..."
        v-model="query"
        @keypress.enter="handleSearch"
        @focus="focused = true"
        @blur="focused = false"
        autocomplete="off"
        spellcheck="false"
      />
      <button v-if="query" class="clear-btn" @click="query = ''; $refs.input.focus()">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 6 6 18M6 6l12 12"/>
        </svg>
      </button>
      <div v-if="loading" class="spin-ring"></div>
      <button v-else class="search-btn" @click="handleSearch" :disabled="!query.trim()">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M12 5l7 7-7 7"/>
        </svg>
      </button>
    </div>
    <p v-if="error" class="error-msg">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="10"/><path d="M12 8v4M12 16h.01"/>
      </svg>
      {{ error }}
    </p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      api_key: "17cbaaac382b9f057f5d10130dd982b8",
      url_base: "https://api.openweathermap.org/data/2.5/",
      lang: "pt_br",
      apiForecast: "35b23f032406442889d125018250302",
      urlForecast: "http://api.weatherapi.com/v1/forecast.json",
      query: "",
      focused: false,
      loading: false,
      error: "",
    };
  },
  methods: {
    async handleSearch() {
      const q = this.query.trim();
      if (!q) return;
      this.loading = true;
      this.error = "";
      try {
        const [current, daily] = await Promise.all([
          axios.get(`${this.url_base}weather?q=${q}&lang=${this.lang}&units=metric&APPID=${this.api_key}`),
          axios.get(`${this.urlForecast}?key=${this.apiForecast}&q=${q}&days=4&aqi=no&alerts=no`),
        ]);
        this.$emit("weather-fetched", current.data);
        this.$emit("forecast-fetched", daily.data);
      } catch {
        this.error = "Cidade não encontrada. Tente outro nome.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.search-wrapper {
  margin: 1.75rem 0 0.5rem;
}

.search-field {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0 1rem;
  height: 54px;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 16px;
  backdrop-filter: blur(12px);
  transition: border-color var(--transition), box-shadow var(--transition), background var(--transition);

  &.focused {
    border-color: rgba(79,163,227,0.45);
    background: rgba(255,255,255,0.07);
    box-shadow: 0 0 0 3px rgba(79,163,227,0.08);
  }
}

.search-icon {
  flex-shrink: 0;
  color: var(--text-muted);
  transition: color var(--transition);

  .focused & { color: var(--accent-cold); }
}

input {
  flex: 1;
  background: none;
  border: none;
  outline: none;
  font-family: var(--font-body);
  font-size: 0.98rem;
  font-weight: 400;
  color: var(--text-primary);
  letter-spacing: 0.01em;

  &::placeholder { color: var(--text-muted); }
}

.clear-btn {
  background: none;
  border: none;
  padding: 4px;
  cursor: pointer;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  border-radius: 6px;
  transition: color var(--transition);
  &:hover { color: var(--text-secondary); }
}

.search-btn {
  flex-shrink: 0;
  width: 34px; height: 34px;
  display: flex; align-items: center; justify-content: center;
  background: var(--accent-cold);
  border: none; border-radius: 10px;
  cursor: pointer; color: #fff;
  transition: opacity var(--transition), transform var(--transition);

  &:hover:not(:disabled) { opacity: 0.85; transform: translateX(1px); }
  &:disabled { opacity: 0.3; cursor: default; }
}

/* Loading spinner */
.spin-ring {
  width: 20px; height: 20px;
  border: 2px solid rgba(79,163,227,0.2);
  border-top-color: var(--accent-cold);
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
  flex-shrink: 0;
}
@keyframes spin { to { transform: rotate(360deg); } }

.error-msg {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  margin-top: 0.65rem;
  padding: 0.55rem 0.9rem;
  font-size: 0.82rem;
  color: #f87171;
  background: rgba(248,113,113,0.08);
  border: 1px solid rgba(248,113,113,0.15);
  border-radius: 10px;
}
</style>