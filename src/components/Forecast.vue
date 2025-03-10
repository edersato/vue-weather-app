<template>
    <div v-if="forecastAvailable" id="cardForecast">
      <div class="forecasts">
        <div 
          v-for="(day, index) in forecastDays" 
          :key="index" 
          class="dayDetails"
        >
          <span>
            <img :src="day.day.condition.icon" alt="IconForecast">
          </span>
          <div class="dayInfo">
            <span>Máxima: {{ Math.round(day.day.maxtemp_c) }}ºC</span>
            <span>Mínima: {{ Math.round(day.day.mintemp_c) }}ºC</span>
            <span>Chance de Chuva: {{ day.day.daily_chance_of_rain }}%</span>
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
        return this.forecast?.forecast?.forecastday || [];
      },
      forecastAvailable() {
        return this.forecastDays.length > 0;
      }
    }
  };
  </script>
  
  <style lang="scss" scoped>
  .forecasts {
    display: flex;
    justify-content: space-between;
    gap: 1.5rem;
    overflow-x: auto;
    padding: 1rem 0;
  }
  
  .dayDetails {
    min-width: 200px;
    text-align: center;
    border-radius: 8px;
    padding: 1rem;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  
    .dayInfo {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
      margin-top: 0.5rem;
    }
  
    img {
      width: 50px;
      height: 50px;
    }
  }
  </style>