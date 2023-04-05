<template>
  <header class="header">
    <div class="header_inner">
      <div class="header_logo">
        <span class="header_logo_text">#demosite</span>
      </div>
      <HeaderNav :class="{'close_header': menu_open, 'open_header': !menu_open}"></HeaderNav>
      <HeaderLang :class="{'close_header': menu_open, 'open_header': !menu_open}"></HeaderLang>
      <MenuButton @menu-open="menuOpen" @menu-close="menuClose"></MenuButton>
    </div>
  </header>
</template>

<script>
import MenuButton from "@/components/Header/MenuButton.vue";
import HeaderNav from "@/components/Header/HeaderNav.vue";
import HeaderLang from "@/components/Header/HeaderLang.vue";

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
  emits: ["menu-open", "menu-close"],
  methods: {
    menuOpen() {
      this.$emit("menu-open");
      this.menu_open = true;
    },
    menuClose() {
      this.$emit("menu-close");
      this.menu_open = false;
    },
  },
};
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #fff;
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
</style>
