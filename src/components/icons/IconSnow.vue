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
    <g class="snow-cloud" aria-hidden="true">
      <circle cx="34" cy="44" r="15" />
      <circle cx="50" cy="38" r="17" />
      <circle cx="66" cy="44" r="13" />
      <rect   x="34" y="44" width="45" height="14" />
    </g>

    <!-- Snowflakes (circles + crosshair) -->
    <g class="flakes" aria-hidden="true">
      <g class="flake f1"><circle cx="34" cy="66" r="2.5"/><line x1="34" y1="61" x2="34" y2="71"/><line x1="29" y1="66" x2="39" y2="66"/></g>
      <g class="flake f2"><circle cx="50" cy="72" r="2.5"/><line x1="50" y1="67" x2="50" y2="77"/><line x1="45" y1="72" x2="55" y2="72"/></g>
      <g class="flake f3"><circle cx="66" cy="66" r="2.5"/><line x1="66" y1="61" x2="66" y2="71"/><line x1="61" y1="66" x2="71" y2="66"/></g>
      <g class="flake f4"><circle cx="42" cy="80" r="2"/><line x1="42" y1="76" x2="42" y2="84"/><line x1="38" y1="80" x2="46" y2="80"/></g>
      <g class="flake f5"><circle cx="58" cy="80" r="2"/><line x1="58" y1="76" x2="58" y2="84"/><line x1="54" y1="80" x2="62" y2="80"/></g>
    </g>
  </svg>
</template>

<script>
export default {
  name: 'IconSnow',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Neve' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.snow-cloud circle,
.snow-cloud rect { fill: #8fa8bf; }

.flake circle { fill: #c8dff0; }
.flake line   { stroke: #c8dff0; stroke-width: 1.8; stroke-linecap: round; }

.flake {
  opacity: 0;
  animation: snow-fall 2s ease-in-out infinite;
}

.f1 { animation-delay: 0.0s; }
.f2 { animation-delay: 0.5s; }
.f3 { animation-delay: 1.0s; }
.f4 { animation-delay: 0.25s; }
.f5 { animation-delay: 0.75s; }

@keyframes snow-fall {
  0%   { transform: translateY(-6px) rotate(0deg);   opacity: 0; }
  20%  { opacity: 1; }
  80%  { opacity: 1; }
  100% { transform: translateY(10px) rotate(45deg);  opacity: 0; }
}
</style>