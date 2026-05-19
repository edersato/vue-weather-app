<template>
  <svg
    :width="size" :height="size"
    viewBox="0 0 100 100"
    xmlns="http://www.w3.org/2000/svg"
    :aria-label="label"
    role="img"
    class="weather-icon"
  >
    <title>{{ label }}</title>

    <!-- Cloud -->
    <g class="rain-cloud" aria-hidden="true">
      <circle cx="34" cy="44" r="15" />
      <circle cx="50" cy="38" r="17" />
      <circle cx="66" cy="44" r="13" />
      <rect   x="34" y="44" width="45" height="14" />
    </g>

    <!-- Raindrops -->
    <g class="drops" aria-hidden="true">
      <line class="drop d1" x1="34" y1="64" x2="30" y2="76" />
      <line class="drop d2" x1="44" y1="64" x2="40" y2="76" />
      <line class="drop d3" x1="54" y1="64" x2="50" y2="76" />
      <line class="drop d4" x1="64" y1="64" x2="60" y2="76" />
      <line class="drop d5" x1="74" y1="64" x2="70" y2="76" />
      <line class="drop d6" x1="39" y1="72" x2="35" y2="84" />
      <line class="drop d7" x1="59" y1="72" x2="55" y2="84" />
      <line class="drop d8" x1="69" y1="72" x2="65" y2="84" />
    </g>
  </svg>
</template>

<script>
export default {
  name: 'IconRain',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Chuva' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.rain-cloud circle,
.rain-cloud rect { fill: #7b96b2; }

.drop {
  stroke: #5ba3d9;
  stroke-width: 2.5;
  stroke-linecap: round;
  opacity: 0;
  animation: rain-fall 1.3s linear infinite;
}

.d1 { animation-delay: 0.00s; }
.d2 { animation-delay: 0.20s; }
.d3 { animation-delay: 0.40s; }
.d4 { animation-delay: 0.60s; }
.d5 { animation-delay: 0.80s; }
.d6 { animation-delay: 0.10s; }
.d7 { animation-delay: 0.50s; }
.d8 { animation-delay: 0.70s; }

@keyframes rain-fall {
  0%   { transform: translateY(-8px);  opacity: 0; }
  15%  { opacity: 1; }
  80%  { opacity: 1; }
  100% { transform: translateY(10px); opacity: 0; }
}
</style>