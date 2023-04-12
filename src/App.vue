<template>
  <CursorDot/>
  <router-view />
</template>

<script>
import CursorDot from "@/components/CursorDot.vue";

export default {
  name: "App",
  components: {
    CursorDot,
  },
};
</script>

<style>
@import "@/assets/variables.css";

body{
  margin: 0;
  padding: 0;
  overflow-x: hidden;
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
