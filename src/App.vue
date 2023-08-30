<template>
  <Home id="home" />
</template>

<script>
import Home from "./views/Home.vue";
export default {
  components: {
    Home,
  },
};
</script>

<style>

* {
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Montserrat', sans-serif;
  background-color: #85c1e9;
}

h1, h2 {
  text-align: center;
  padding-top: 1em;
  color: black;
  font-weight: 300;
}

</style>