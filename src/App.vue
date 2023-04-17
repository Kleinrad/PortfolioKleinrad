<template>
  <router-view />
</template>

<script>

export default {
  name: "App",
  components: {
  },
};
</script>

<style>
@import "@/assets/variables.css";

body {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  background-color: black;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: white;
  color: var(--font-color);
}
</style>
