<template>
  <div class="weather-wrap" v-if="weatherAvailable">
    <div class="location-box">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="date">{{ dateBuilder() }}</div>
    </div>

    <div class="weather-box">
      <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
      <div class="weather-desc">{{ weather.weather[0].description }}</div>
      <div class="variations">
        <div class="min-temp">
          <h6>{{ min }}</h6>
          {{ Math.round(weather.main.temp_min) }}°C
        </div>
        <div class="max-temp">
          <h6>{{ max }}</h6>
          {{ Math.round(weather.main.temp_max) }}°C
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeatherDisplay",
  props: {
    weather: Object,
  },
  data() {
    return {
      min: "Mínima",
      max: "Máxima",
      feels: "Sensação Térmica",
    };
  },
  methods: {
    dateBuilder() {
      let d = new Date();
      let months = [
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
      let days = [
        "Domingo",
        "Segunda",
        "Terça",
        "Quarta",
        "Quinta",
        "Sexta",
        "Sábado",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    setForecast(forecastData) {
      this.forecast = forecastData;
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
h6 {
  padding-bottom: 0.2em;
  font-weight: 300;
}

.location-box {
    padding: 3em 0;
  .location {
    padding-top: 2rem;
    color: #000;
    font-size: 30px;
    font-weight: 500;
    text-align: center;
    margin-bottom: 5px;
  }

  .date {
    color: #000;
    font-size: 17px;
    font-weight: 300;
    font-style: bold;
    text-align: center;
  }
}

.weather-box {
  text-align: center;
  min-height: 500px;
  padding: 1em 0;

  .temperature {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
  }

  .temp {
    display: inline-block;
    padding: 5px 20px;
    color: #000;
    font-size: 80px;
    font-weight: 900;

    text-shadow: 3px 6px #ffffff40;
    background-color: #ffffff40;
    border-radius: 16px;
    margin: 30px 0;

    box-shadow: 3px 6px #00000040;
  }

  .variations {
    display: flex;
    justify-content: space-evenly;
    padding-top: 2em;
    padding-bottom: 5em;
  }

  .min-temp,
  .max-temp,
  .feels {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 5px 20px;
    color: #000;
    font-size: 50px;
    font-weight: 700;
    box-shadow: 3px 6px #00000040;
    border-radius: 16px;
  }

  .min-temp {
    color: #00193f;
    background-color: #60acdfb4;
  }

  .feels {
    background-color: #dadada73;
  }

  .max-temp {
    color: #3f1900;
    background-color: #ff4400a2;
  }

  .weather-desc {
    color: #000;
    font-size: 36px;
    text-transform: capitalize;
    font-style: italic;
    margin: 1.5em 0;
  }
}
</style>
