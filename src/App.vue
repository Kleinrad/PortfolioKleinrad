<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
  <CursorDot/>
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

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
}
</style>
