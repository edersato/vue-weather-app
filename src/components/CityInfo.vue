<template>
  <div v-if="weatherDefine" id="card">
    <div class="heading-info">
      <img :src="weatherImg" alt="Icons" />
      <div class="temp">
        <div class="main-temp">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
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
        <div class="humidity">
          <span>Umidade: {{ weather.main.humidity }}%</span>
        </div>
      </div>
    </div>
  </div>
  <div v-if="weatherDefine">
    <Forecast :forecast="forecast" />
  </div>
</template>

<script>
import Forecast from './Forecast.vue';
export default {
  props: ["weather", "forecast"],
  components: { Forecast },
  methods: {
    formatDate(d) {
      const days = [
        "Domingo",
        "Segunda-feira",
        "Terça-feira",
        "Quarta-feira",
        "Quinta-feira",
        "Sexta-feira",
        "Sábado",
      ];
      const months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
      ];
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

    forecastAvailable() {
      return this.forecast?.forecast?.forecastday?.length > 0;
    },

    weatherDefine() {
      return this.weatherAvailable && this.forecast;
    },

    weatherImg() {
      var description = this.weather.weather[0].description.toLowerCase();
      if (description.includes("limpo") || description.includes("sol")) {
        return "icons/001-sun.png";
      } else if (
        description.includes("nublado") ||
        description.includes("nuvens") ||
        description.includes("nuvem")
      ) {
        return "icons/002-cloudy.png";
      } else if (description.includes("chuva")) {
        return "icons/003-rain.png";
      } else if (
        description.includes("tempestade") ||
        description.includes("trovoadas")
      ) {
        return "icons/004-storm.png";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=New+Amsterdam&family=Tajawal:wght@200;300;400;500;700;800;900&display=swap");

#card,
#cardForecast {
  background-color: #d2d2d2;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 2rem 3rem;
  margin: 1.5rem;
  text-align: center;
}

.heading-info {
  display: grid;
  grid-template-columns: repeat(2, 50%);
  font-family: "Tajawal", serif;

  img {
    width: 150px;
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
        font-weight: 300;
      }

      .location {
        font-size: 1.5rem;
        font-weight: 500;
      }
    }

    .minMax {
      display: flex;
      flex-direction: column;
      font-size: 1.2rem;
      padding: 1.2rem;
      font-weight: 300;

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
  font-family: "Tajawal", serif;

  .description {
    font-size: 2rem;
    text-transform: capitalize;
    font-weight: 400;
  }

  .anotherI {
    display: flex;
    flex-direction: column;
  }

  .humidity {
    font-size: 2rem;
    font-weight: 300;
  }
}
</style>
