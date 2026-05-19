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

    <!-- Dark storm cloud -->
    <g class="storm-cloud" aria-hidden="true">
      <circle cx="32" cy="40" r="15" />
      <circle cx="50" cy="34" r="18" />
      <circle cx="67" cy="40" r="13" />
      <rect   x="32" y="40" width="48" height="14" />
    </g>

    <!-- Lightning bolt -->
    <polygon class="bolt" points="53,56 44,72 51,72 46,88 62,68 55,68" aria-hidden="true" />

    <!-- Side raindrops -->
    <g class="side-drops" aria-hidden="true">
      <line class="sdrop s1" x1="26" y1="60" x2="22" y2="72" />
      <line class="sdrop s2" x1="34" y1="68" x2="30" y2="80" />
      <line class="sdrop s3" x1="72" y1="60" x2="68" y2="72" />
      <line class="sdrop s4" x1="80" y1="68" x2="76" y2="80" />
    </g>
  </svg>
</template>

<script>
export default {
  name: 'IconStorm',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Tempestade' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.storm-cloud circle,
.storm-cloud rect { fill: #3d4f63; }

/* Bolt flickers irregularly */
.bolt {
  fill: #f9d349;
  filter: drop-shadow(0 0 6px rgba(249,211,73,0.8));
  animation: bolt-flicker 2.4s ease-in-out infinite;
  transform-origin: 53px 72px;
}
@keyframes bolt-flicker {
  0%,38%,42%,58%,62%,100% { opacity: 1;   transform: scaleX(1);   }
  40%                      { opacity: 0.1; transform: scaleX(0.98); }
  60%                      { opacity: 0.15; }
}

.sdrop {
  stroke: #7ec8e3;
  stroke-width: 2.2;
  stroke-linecap: round;
  opacity: 0;
  animation: rain-fall 1.5s linear infinite;
}
.s1 { animation-delay: 0.00s; }
.s2 { animation-delay: 0.55s; }
.s3 { animation-delay: 0.25s; }
.s4 { animation-delay: 0.80s; }

@keyframes rain-fall {
  0%   { transform: translateY(-8px);  opacity: 0; }
  15%  { opacity: 0.85; }
  80%  { opacity: 0.85; }
  100% { transform: translateY(10px); opacity: 0; }
}
</style>