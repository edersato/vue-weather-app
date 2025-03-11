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
          <span class="temp">Max: {{ Math.round(hour.temp_c) }}°C</span>
          <span class="rain">Umidade: {{ hour.chance_of_rain }}%</span>
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
  
  .hourly-container {
    display: grid;
    grid-template-columns: repeat(4, 23%);
    gap: 2%;
    overflow-y: auto;
  }
  
  .hour-card {
    display: flex;
    flex-direction: column;
    text-align: center;
    background: white;
    border-radius: 8px;
    padding: 0.5rem;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  
    .time {
      font-size: 0.9rem;
      font-weight: 700;
      display: block;
      margin-bottom: 0.5rem;
    }

    img {
      width: 40px;
      height: 40px;
      align-self: center;
      margin-bottom: 1em;
    }
  
    .temp {
      font-weight: bold;
      margin-bottom: .5rem;
    }
  
    .rain {
      color: #2c3e50;
      font-size: 0.8rem;
    }
  }
  </style>

  <style lang="scss" scoped>
  
  @media(min-width: 300px) and (max-width: 425px) {
    .hourly-container {
        grid-template-columns: auto;
        overflow-y: auto;
        gap: 1em;
        height: 300px;
    }
  }

  @media(min-width: 426px) and (max-width: 600px) {
    .hourly-container {
        grid-template-columns: repeat(2, 48%);
    }
  }
  </style>