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
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  overflow-x: hidden;
  background-color: var(--background-color);
  -webkit-transition: var(--bg-transition);
  -o-transition: var(--bg-transition);
  transition: var(--bg-transition);
  -ms-scroll-chaining: none;
      overscroll-behavior: contain;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: white;
  color: var(--font-color);
}

a {
  color: var(--accent-color);
  font-weight: 600;
  text-decoration: none;
}
</style>
