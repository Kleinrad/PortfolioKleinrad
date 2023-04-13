<template>
  <HeaderBar @menu-close="showMenu=false" @menu-open="showMenu=true"></HeaderBar>
  <MenuFull v-if="showMenu"></MenuFull>
  <Transition name="fade">
    <div class="mainPage" v-if="!showMenu">
      <HomeBanner></HomeBanner>
      <ProjectsOverview></ProjectsOverview>
    </div>
  </Transition>
</template>

<script>
import HeaderBar from "@/components/Header/HeaderBar.vue";
import MenuFull from "@/components/MenuFull.vue";
import HomeBanner from "@/components/Home/HomeBanner.vue";
import ProjectsOverview from "@/components/Home/Projects/ProjectsOverview.vue";

export default {
  name: "HomeView",
  data() {
    return {
      showMenu: false,
    };
  },
  components: {
    HeaderBar,
    MenuFull,
    HomeBanner,
    ProjectsOverview,
  },
};
</script>

<style scoped>

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

HomeBanner {
  height: 120svh;
}

HeaderBar {
  position: fixed;
  z-index: 10;
}

MenuFull {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 5;
}

.fade-out {
  animation: fadeOut .2s forwards;
}

@keyframes fadeOut {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }  
}
</style>
