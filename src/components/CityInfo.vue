<template>
  <div v-if="weatherAvailable" class="card">
    <div class="location-box">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
    </div>
    <div class="weather-info">
      <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
      <div class="description">{{ weather.weather[0].description }}</div>
      <div class="temp-range">
        <div class="min">
          <span>Min: {{ Math.round(weather.main.temp_min) }}°C</span>
        </div>
        <div class="max">
          <span>Max: {{ Math.round(weather.main.temp_max) }}°C</span>
        </div>
      </div>
      <div class="additional-info">
        <div class="wind">
          <span>Velocidade do Vento: {{ weather.wind.speed }} m/s</span>
        </div>
        <div class="humidity">
          <span>Humidade: {{ weather.main.humidity }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["weather", "forecast"],
  methods: {
    formatDate(d) {
      const days = ["Domingo", "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sábado"];
      const months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
  computed: {
    weatherAvailable() {
      return typeof this.weather.main !== "undefined";
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  background-color: linear-gradient(transparent, #d2d2d2);
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  margin: 1rem 4.5em;
  text-align: center;
}

.location-box {
  margin-bottom: 3rem;

  .location {
    font-size: 2rem;
    font-weight: 600;
  }
}

.weather-info {
  .temp {
    font-size: 3rem;
    font-weight: 700;
    
    .description {
      font-size: 1rem;
      margin: 0.5rem 0;
      border-bottom: 1px solid #fff;
      padding-bottom: 0.5em;
    }
  }
  .temp-range {
    padding: 0.8rem 0;
    font-size: 1.5rem;
    color: #666;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #fff;
    margin: 0.5rem 0;

    .min, .max {
      border: 1px solid #fff;
      border-radius: 8px;
      padding: 2rem;
    }
  }
  .additional-info {
    font-size: 1.2rem;
    color: #666;
    display: flex;
    justify-content: space-between;
    margin: 0.5rem 0;

    .wind, .humidity {
      border: 1px solid #fff;
      border-radius: 9px;
      padding: 1.5rem;
    }

  }
}
</style>