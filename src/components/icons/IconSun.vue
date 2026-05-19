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
    <g class="rays" aria-hidden="true">
      <line x1="50" y1="6"  x2="50" y2="20" />
      <line x1="50" y1="80" x2="50" y2="94" />
      <line x1="6"  y1="50" x2="20" y2="50" />
      <line x1="80" y1="50" x2="94" y2="50" />
      <line x1="18" y1="18" x2="28" y2="28" />
      <line x1="72" y1="72" x2="82" y2="82" />
      <line x1="82" y1="18" x2="72" y2="28" />
      <line x1="18" y1="82" x2="28" y2="72" />
    </g>
    <circle class="core" cx="50" cy="50" r="16" aria-hidden="true" />
  </svg>
</template>

<script>
export default {
  name: 'IconSun',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Céu limpo' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.rays {
  transform-origin: 50px 50px;
  animation: sun-spin 10s linear infinite;
}
.rays line {
  stroke: #f9b233;
  stroke-width: 3;
  stroke-linecap: round;
}

.core {
  fill: #f9b233;
  transform-origin: 50px 50px;
  animation: sun-pulse 3s ease-in-out infinite;
  filter: drop-shadow(0 0 8px rgba(249,178,51,0.55));
}

@keyframes sun-spin  { to { transform: rotate(360deg); } }
@keyframes sun-pulse {
  0%, 100% { r: 16px; }
  50%       { r: 19px; }
}
</style>