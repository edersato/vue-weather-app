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
@import url('https://fonts.googleapis.com/css2?family=New+Amsterdam&display=swap');


* {
  margin: 0;
  padding: 0;
}

h1, h2, h3 {
  font-family: "New Amsterdam", serif;
  font-weight: 400;
  font-style: normal;
}

body {
  font-family: 'Montserrat', sans-serif;
  background-color: #d4d4d4;
}

h1, h2 {
  text-align: center;
  padding-top: 1em;
  color: black;
  font-weight: 300;
}

h3 {
  letter-spacing: 1.2px;
}

.card-container {
  background-color: #d2d2d2;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 2rem 3rem;
  margin: 1.5rem;
  text-align: center;
}

</style>