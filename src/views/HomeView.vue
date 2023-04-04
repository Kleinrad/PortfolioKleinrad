<template>
  <div>
    <HeaderBar @menu-close="showMenu=false" @menu-open="showMenu=true"></HeaderBar>
    <MenuFull v-if="showMenu"></MenuFull>
    <div class="main_page">
      <HomeBanner></HomeBanner>
    </div>
  </div>
</template>

<script>
import HeaderBar from "@/components/Header/HeaderBar.vue";
import MenuFull from "@/components/MenuFull.vue";
import HomeBanner from "@/components/HomeBanner.vue";

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
  },
};
</script>

<style scoped>

HeaderBar {
  position: fixed;
  z-index: 1;
}

MenuFull {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 0;
}
</style>
