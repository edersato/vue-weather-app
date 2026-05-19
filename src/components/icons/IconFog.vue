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

    <!-- Top cloud silhouette -->
    <g class="fog-cloud" aria-hidden="true">
      <circle cx="36" cy="36" r="14" />
      <circle cx="52" cy="30" r="16" />
      <circle cx="67" cy="36" r="12" />
      <rect   x="36" y="36" width="43" height="12" />
    </g>

    <!-- Fog lines -->
    <line class="fline l1" x1="18" y1="56" x2="82" y2="56" aria-hidden="true" />
    <line class="fline l2" x1="22" y1="66" x2="78" y2="66" aria-hidden="true" />
    <line class="fline l3" x1="28" y1="76" x2="72" y2="76" aria-hidden="true" />
  </svg>
</template>

<script>
export default {
  name: 'IconFog',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Névoa / Neblina' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.fog-cloud circle,
.fog-cloud rect { fill: #8fa8bf; }

.fline {
  stroke: #8fa8bf;
  stroke-width: 3.5;
  stroke-linecap: round;
  animation: fog-slide 3s ease-in-out infinite;
}

.l1 { animation-delay: 0.0s; transform-origin: 50px 56px; }
.l2 { animation-delay: 0.4s; transform-origin: 50px 66px; animation-direction: alternate-reverse; }
.l3 { animation-delay: 0.8s; transform-origin: 50px 76px; }

@keyframes fog-slide {
  0%,100% { transform: translateX(0px);  opacity: 0.55; }
  50%     { transform: translateX(5px);  opacity: 1;    }
}
</style>