<template>
  <header class="header">
    <div class="header_inner">
      <div class="header_logo">
        <span class="header_logo_text">/&lt;leinrad</span>
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
    };
  },
  components: {
    MenuButton,
    HeaderNav,
    HeaderLang,
  },
  props: {
      menu_open: Boolean,
  },
  emits: ["menu-open", "menu-close", "scroll-projects", "scroll-about"],
  methods: {
    menuOpen() {
      this.$emit("menu-open");
    },
    menuClose() {
      this.$emit("menu-close");
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
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  width: 100%;
  background-color: var(--background-color);
  -webkit-transition: var(--bg-transition);
  -o-transition: var(--bg-transition);
  transition: var(--bg-transition);
  z-index: 10;
}

.header_inner {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
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
  -webkit-animation: slide-close 0.5s ease-in-out;
          animation: slide-close 0.5s ease-in-out;
  -webkit-transition: visibility 0s 0.5s;
  -o-transition: visibility 0s 0.5s;
  transition: visibility 0s 0.5s;
}

@-webkit-keyframes slide-close {
  0% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
    width: -webkit-fit-content;
    width: fit-content;
  }
  100% {
    width: 0px;
    opacity: 0;
    -webkit-transform: translateX(10%);
            transform: translateX(10%);
  }
}

@keyframes slide-close {
  0% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
    width: -webkit-fit-content;
    width: -moz-fit-content;
    width: fit-content;
  }
  100% {
    width: 0px;
    opacity: 0;
    -webkit-transform: translateX(10%);
            transform: translateX(10%);
  }
}

.open_header {
  -webkit-animation: slide-open 0.5s forwards;
          animation: slide-open 0.5s forwards;
}

@-webkit-keyframes slide-open {
  0% {
    width: 0px;
    opacity: 0;
    -webkit-transform: translateX(10%);
            transform: translateX(10%);
  }
  100% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
    width: -webkit-fit-content;
    width: fit-content;
  }
}

@keyframes slide-open {
  0% {
    width: 0px;
    opacity: 0;
    -webkit-transform: translateX(10%);
            transform: translateX(10%);
  }
  100% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
    width: -webkit-fit-content;
    width: -moz-fit-content;
    width: fit-content;
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
</style>
