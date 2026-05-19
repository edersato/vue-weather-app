<template>
  <component :is="iconComponent" :size="size" />
</template>

<script>
import IconSun          from './icons/IconSun.vue'
import IconPartlyCloudy from './icons/IconPartlyCloudy.vue'
import IconCloudy       from './icons/IconCloudy.vue'
import IconRain         from './icons/IconRain.vue'
import IconStorm        from './icons/IconStorm.vue'
import IconFog          from './icons/IconFog.vue'
import IconSnow         from './icons/IconSnow.vue'

export default {
  name: 'WeatherIcon',
  components: { IconSun, IconPartlyCloudy, IconCloudy, IconRain, IconStorm, IconFog, IconSnow },

  props: {
    /* OpenWeather condition code (weather[0].id) */
    code: { type: Number, required: true },
    size: { type: Number, default: 80 },
  },

  computed: {
    iconComponent() {
      const c = this.code
      if (c >= 200 && c < 300) return 'IconStorm'
      if (c >= 300 && c < 600) return 'IconRain'
      if (c >= 600 && c < 700) return 'IconSnow'
      if (c >= 700 && c < 800) return 'IconFog'
      if (c === 800)            return 'IconSun'
      if (c === 801 || c === 802) return 'IconPartlyCloudy'
      return 'IconCloudy'
    },
  },
}
</script>