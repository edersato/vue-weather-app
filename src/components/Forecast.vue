<template>
  <div v-if="forecastAvailable" id="cardForecast" class="card-container">
    <div class="forecasts">
      <div 
        v-for="(day, index) in forecastDays" 
        :key="index" 
        class="day-block"
      >
        <h3 class="day-title">{{ getDayLabel(day.date, index) }}</h3>
        <div class="dayDetails">
          <span>
            <img :src="day.day.condition.icon" alt="IconForecast">
          </span>
          <div class="dayInfo">
            <span>Max: {{ Math.round(day.day.maxtemp_c) }}ºC</span>
            <span>Min: {{ Math.round(day.day.mintemp_c) }}ºC</span>
            <span>Chance de Chuva: {{ day.day.daily_chance_of_rain }}%</span>
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
      return this.forecast?.forecast?.forecastday || [];
    },
    forecastAvailable() {
      return this.forecastDays.length > 0;
    }
  },
  methods: {
    getDayLabel(dateString, index) {
      const today = new Date();
      const targetDate = new Date(dateString);
      
      const diffDays = Math.floor((targetDate - today) / (1000 * 60 * 60 * 24));
      
      switch (diffDays) {
        case 0:
          return "Hoje";
        case 1:
          return "Amanhã";
        default:
          return targetDate.toLocaleDateString("pt-BR", { 
            weekday: "long", 
            month: "numeric", 
            day: "numeric" 
          });
      }
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

  <style lang="scss" scoped>
  @media(min-width: 300px) and (max-width: 650px) {
    .forecasts {
      flex-direction: column;
    }
  }
  </style>