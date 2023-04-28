<template>
    <div class="header_nav">
      <span @click="scrollProjects">projects</span>
      <span @click="scrollAbout">about me</span>
    </div>
</template>

<script>
  export default {
    name: "HeaderNav",
    emits: ["scroll-projects", "scroll-about"],
    methods: {
      scrollProjects() {
        this.$emit("scroll-projects");
      },
      scrollAbout() {
        this.$emit("scroll-about");
      },
    },
  };
</script>

<style scoped>

.header_nav {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: space-evenly;
      -ms-flex-pack: space-evenly;
          justify-content: space-evenly;
  width: 30vw;
  min-width: -webkit-fit-content;
  min-width: -moz-fit-content;
  min-width: fit-content;
  font-size: 1vw;
  font-weight: 600;
}

.header_nav > span {
  margin: 0 3vw;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: end;
      -ms-flex-align: end;
          align-items: flex-end;
  cursor: pointer;
}

@media screen and (max-width: 768px) {
  .header_nav {
    display: none;
  }
}

@media screen and (max-width: 1050px) {
  .header_nav {
    width: 50vw;
    font-size: 1.8vw;
  }

  .header_nav > span {
    margin: 0 2vw;
  }
}
</style>