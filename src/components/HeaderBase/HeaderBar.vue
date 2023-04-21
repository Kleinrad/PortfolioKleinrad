<template>
  <header class="header">
    <div class="header_inner">
      <div class="header_logo">
        <span class="header_logo_text">#demosite</span>
      </div>
      <HeaderNav :class="{'close_header': menu_open, 'open_header': !menu_open}"
        @scroll-projects="scrollProjects"
        @scroll-about="scrollAbout"
      ></HeaderNav>
      <HeaderLang :class="{'close_header': menu_open, 'open_header': !menu_open}"></HeaderLang>
      <MenuButton @menu-open="menuOpen" @menu-close="menuClose"></MenuButton>
    </div>
  </header>
</template>

<script>
import MenuButton from "@/components/HeaderBase/MenuButton.vue";
import HeaderNav from "@/components/HeaderBase/HeaderNav.vue";
import HeaderLang from "@/components/HeaderBase/HeaderLang.vue";

export default {

  name: "HeaderBar",
  data() {
    return {
      menu_open: false,
    };
  },
  components: {
    MenuButton,
    HeaderNav,
    HeaderLang,
  },
  emits: ["menu-open", "menu-close", "scroll-projects", "scroll-about"],
  methods: {
    menuOpen() {
      this.$emit("menu-open");
      this.menu_open = true;
    },
    menuClose() {
      this.$emit("menu-close");
      this.menu_open = false;
    },
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
.header {
  position: fixed;
  top: 0;
  left: 0;
  box-sizing: border-box;
  width: 100%;
  background-color: var(--background-color);
  transition: var(--bg-transition);
  z-index: 10;
}

.header_inner {
  display: flex;
  align-items: center;
  height: 100%;
  padding: 3vw 5vw 1.2vw;
}

.header_logo {
  margin-right: auto;
  font-weight: 700;
  font-size: 1.5vw;
}

.close_header {
  visibility: hidden;
  animation: slide-close 0.5s ease-in-out;
  transition: visibility 0s 0.5s;
}

@keyframes slide-close {
  0% {
    transform: translateX(0);
    width: fit-content;
  }
  100% {
    width: 0px;
    opacity: 0;
    transform: translateX(10%);
  }
}

.open_header {
  animation: slide-open 0.5s forwards;
}

@keyframes slide-open {
  0% {
    width: 0px;
    opacity: 0;
    transform: translateX(10%);
  }
  100% {
    transform: translateX(0);
    width: fit-content;
  }
}

@media screen and (max-width: 768px) {
  .header {
    padding: 2vw 3vw;
  }
  .header_inner {
    padding: 2vw 3vw;
  }
  .header_logo {
    font-size: 4.5vw;
  }
}

@media screen and (max-width: 1050px) {
  .header {
    padding: 1.5vw 2.5vw;
  }
  .header_inner {
    padding: 1.5vw 2.5vw;
  }
  .header_logo {
    font-size: 2vw;
  }
}
</style>
