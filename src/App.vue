<template>
  <notes></notes>
</template>

<script>
import notes from "./components/notes.vue";


export default {
  name: "App",
  components: {
    notes
  },
};
</script>

<style>
body {
  font-family: "Itim", sans-serif ;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  margin: 0;
  background-color: #faf6d2;
}
#app {
  -webkit-font-smoothing: antialiased;
  font-family: "Itim", sans-serif;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #faf6d2;
  max-width: 1280px;
  min-height: 100vh;
  margin: auto;
}
</style>
