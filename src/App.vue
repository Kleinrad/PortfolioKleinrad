<template>
  <CursorDot/>
  <router-view />
</template>

<script>
import CursorDot from "@/components/CursorDot.vue";

export default {
  name: "App",
  data() {
    return {
      scrollY: 0,
      scrollCounter: 0,
      accentColor: ["#4eeed9", "#A1CEB3"],
      secondaryColor: ["#97b1bb", "#1B2626"],
      backgroundColor: ["#fff", "#080E0E"],
      fontColor: ["#000", "#EFFBF3"],
    };
  },
  components: {
    CursorDot,
  },
  methods: {
    handleScroll() {
      this.scrollY = window.scrollY;
      if (!this.isDark && this.scrollY > 550) {
        this.isDark = true;
        document.documentElement.style.setProperty("--accent-color", this.accentColor[1]);
        document.documentElement.style.setProperty("--secondary-color", this.secondaryColor[1]);
        document.documentElement.style.setProperty("--background-color", this.backgroundColor[1]);
        document.documentElement.style.setProperty("--font-color", this.fontColor[1]);
      }else if (this.isDark && this.scrollY < 350) {
        this.isDark = false;
        document.documentElement.style.setProperty("--accent-color", this.accentColor[0]);
        document.documentElement.style.setProperty("--secondary-color", this.secondaryColor[0]);
        document.documentElement.style.setProperty("--background-color", this.backgroundColor[0]);
        document.documentElement.style.setProperty("--font-color", this.fontColor[0]);
      }
    },
    scrollIntercept(e) {
      e.preventDefault();
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
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
