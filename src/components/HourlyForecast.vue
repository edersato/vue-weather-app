<template>
    <div v-if="hourlyForecastAvailable" class="card-container">
      <h2>Previsão Horária</h2>
      <div class="hourly-container">
        <div 
          v-for="(hour, index) in filteredHours" 
          :key="index" 
          class="hour-card"
        >
          <span class="time">{{ formatTime(hour.time) }}</span>
          <img :src="hour.condition.icon" alt="Hour Icon">
          <span class="temp">{{ Math.round(hour.temp_c) }}°C</span>
          <span class="rain">{{ hour.chance_of_rain }}%</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ["forecast"],
    computed: {
      // Filtra as horas do dia atual
      filteredHours() {
        const today = new Date().toISOString().split('T')[0]; // Formato YYYY-MM-DD
        return this.forecast?.forecast?.forecastday
          ?.find(day => day.date === today)?.hour || [];
      },
      hourlyForecastAvailable() {
        return this.filteredHours.length > 0;
      }
    },
    methods: {
      formatTime(time) {
        return new Date(time).toLocaleTimeString('pt-BR', { 
          hour: '2-digit', 
          minute: '2-digit' 
        });
      }
    }
  };
  </script>
  
  <style lang="scss" scoped>

  h2 {
    padding-bottom: 1.5rem;
  }

  .hourly-forecast {
    margin-top: 2rem;
    padding: 1rem;
    background: #f5f5f5;
    border-radius: 8px;
  
    h3 {
      font-family: 'Tajawal', sans-serif;
      margin-bottom: 1rem;
    }
  }
  
  .hourly-container {
    display: flex;
    gap: 1rem;
    overflow-x: auto;
    padding-bottom: 1rem;
  }
  
  .hour-card {
    min-width: 80px;
    text-align: center;
    background: white;
    border-radius: 8px;
    padding: 0.5rem;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  
    img {
      width: 40px;
      height: 40px;
      display: block;
      margin: 0 auto;
    }
  
    .time {
      font-size: 0.9rem;
      display: block;
      margin-bottom: 0.5rem;
    }
  
    .temp {
      font-weight: bold;
    }
  
    .rain {
      color: #2c3e50;
      font-size: 0.8rem;
    }
  }
  </style>