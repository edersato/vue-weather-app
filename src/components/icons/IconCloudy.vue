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

    <!-- Back cloud (darker, slower) -->
    <g class="cloud-back" aria-hidden="true">
      <circle cx="30" cy="52" r="13" />
      <circle cx="45" cy="45" r="15" />
      <circle cx="62" cy="50" r="13" />
      <rect   x="30" y="52" width="45" height="16" />
    </g>

    <!-- Front cloud (lighter, faster) -->
    <g class="cloud-front" aria-hidden="true">
      <circle cx="38" cy="62" r="14" />
      <circle cx="55" cy="56" r="16" />
      <circle cx="70" cy="62" r="12" />
      <rect   x="38" y="62" width="44" height="15" />
    </g>
  </svg>
</template>

<script>
export default {
  name: 'IconCloudy',
  props: { size: { type: Number, default: 80 } },
  computed: { label() { return 'Nublado' } },
}
</script>

<style scoped>
.weather-icon { overflow: visible; display: block; }

.cloud-back circle,
.cloud-back rect {
  fill: #a0b4c8;
  animation: drift-back 5s ease-in-out infinite;
  transform-origin: 47px 54px;
}

.cloud-front circle,
.cloud-front rect {
  fill: #dce8f5;
  animation: drift-front 4s ease-in-out infinite;
  transform-origin: 56px 64px;
}

@keyframes drift-back {
  0%,100% { transform: translateX(0px); }
  50%     { transform: translateX(-4px); }
}
@keyframes drift-front {
  0%,100% { transform: translateX(0px); }
  50%     { transform: translateX(4px);  }
}
</style>