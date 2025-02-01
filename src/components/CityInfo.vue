<template>
  <div v-if="weatherAvailable" class="card">
    <div class="heading-info">
      <img :src="weatherImg" alt="Icons">
      <div class="temp">
        <div class="main-temp">
        <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      </div>
          <div class="minMax">
            <span>Min: {{ Math.round(weather.main.temp_min) }}°C</span>
            <span>Max: {{ Math.round(weather.main.temp_max) }}°C</span>
          </div>
      </div>
    </div>
    <div class="additional-info">
      <div class="description">{{ weather.weather[0].description }}</div>
      <div class="anotherI">
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
    weatherImg() {
      var description = this.weather.weather[0].description.toLowerCase();
      if(description.includes("limpo" || "sol")) {
      return "src/assets/icons/001-sun.png";
      } else if(description.includes("nublado" || "nuvens" || "nuvem")) {
      return "src/assets/icons/002-cloudy.png";
      } else if(description.includes("chuva")) {
      return "src/assets/icons/003-rain.png";
      } else if(description.includes("tempestade")) {
      return "src/assets/icons/004-storm.png";
      }
    }
  },
};
</script>

<style lang="scss" scoped>
.card {
  background-color: linear-gradient(transparent, #d2d2d2);
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  margin: 1rem 2rem;
  text-align: center;
}

.heading-info {
  display: grid;
  grid-template-columns: repeat(2, 50%);

  img {
    width: 300px;
    justify-self: center;
  }

  .temp {
    display: flex;
    padding: 1rem;
    margin-left: 3rem;
    justify-self: flex-end;

    .main-temp {
      display: flex;
      flex-direction: column;
      justify-content: space-between;

      .temperature {
        font-size: 5rem;
        margin-right: 0rem;
      }

      .location {
        font-size: 1.5rem;
      }
    }

    .minMax {
      display: flex;
      flex-direction: column;
      font-size: 1.2rem;
      padding: 1.2rem;

      span:first-child {
        margin-bottom: 0.5rem;
      }
    }
  }
}

.additional-info {
  display: flex;
  justify-content: space-around;
  margin-top: 2rem;

  .description {
    font-size: 2rem;
    text-transform: capitalize;
  }

  .anotherI {
    display: flex;
    flex-direction: column;
  }

  .wind, .humidity {
    font-size: 1.1rem;
  }
}
</style>