<template>
  <Home id="home" />
</template>

<script>
import Home from "./views/Home.vue";
export default {
  components: { Home },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@200;300;400;500;600;700&family=Syne:wght@400;500;600;700;800&display=swap');

:root {
  --bg-deep:       #080c14;
  --bg-mid:        #0d1422;
  --glass-bg:      rgba(255,255,255,0.04);
  --glass-border:  rgba(255,255,255,0.08);
  --glass-hover:   rgba(255,255,255,0.07);
  --accent-cold:   #4fa3e3;
  --accent-hot:    #f97f3b;
  --accent-glow:   #4fa3e3;
  --text-primary:  #eef2ff;
  --text-secondary:#8898b3;
  --text-muted:    #4a5a75;
  --radius-card:   20px;
  --radius-sm:     12px;
  --font-display:  'Syne', sans-serif;
  --font-body:     'Outfit', sans-serif;
  --transition:    0.35s cubic-bezier(0.4,0,0.2,1);
}

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html { scroll-behavior: smooth; }

body {
  font-family: var(--font-body);
  background-color: var(--bg-deep);
  color: var(--text-primary);
  -webkit-font-smoothing: antialiased;
  min-height: 100vh;
}

h1, h2, h3, h4 {
  font-family: var(--font-display);
  font-weight: 600;
  letter-spacing: -0.02em;
}

::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--text-muted); border-radius: 4px; }

/* Utility */
.glass {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-radius: var(--radius-card);
}
</style>