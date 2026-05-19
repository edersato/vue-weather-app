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

    <!-- Sun behind the cloud -->
    <g class="sun-group" aria-hidden="true">
      <g class="sun-rays">
        <line x1="28" y1="10" x2="28" y2="20" />
        <line x1="28" y1="46" x2="28" y2="56" />
        <line x1="8"  y1="33" x2="18" y2="33" />
        <line x1="48" y1="33" x2="58" y2="33" />
        <line x1="14" y1="19" x2="21" y2="26" />
        <line x1="35" y1="40" x2="42" y2="47" />
        <line x1="42" y1="19" x2="35" y2="26" />
        <line x1="14" y1="47" x2="21" y2="40" />
      </g>
      <circle class="sun-core" cx="28" cy="33" r="10" />
    </g>

    <!-- Cloud -->
    <g class="cloud-group" aria-hidden="true">
      <circle cx="42" cy="60" r="15" />
      <circle cx="58" cy="55" r="13" />
      <circle cx="71" cy="60" r="11" />
      <rect   x="42" y="60" width="40" height="16" />
    </g>
  </svg>
</template>

<script>
export default {
  name: 'IconPartlyCloudy',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Parcialmente nublado' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

/* Sun */
.sun-rays {
  transform-origin: 28px 33px;
  animation: sun-spin 12s linear infinite;
}
.sun-rays line {
  stroke: #f9b233;
  stroke-width: 2.5;
  stroke-linecap: round;
}
.sun-core {
  fill: #f9b233;
  transform-origin: 28px 33px;
  animation: sun-pulse 3.5s ease-in-out infinite;
}

/* Cloud */
.cloud-group circle,
.cloud-group rect {
  fill: #dce8f5;
  animation: cloud-drift 4s ease-in-out infinite;
  transform-origin: 57px 64px;
}

@keyframes sun-spin  { to { transform: rotate(360deg); } }
@keyframes sun-pulse { 0%,100%{r:10px} 50%{r:12px} }
@keyframes cloud-drift {
  0%,100% { transform: translateX(0px); }
  50%     { transform: translateX(4px);  }
}
</style>