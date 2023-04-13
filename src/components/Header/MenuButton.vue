<template>
    <button class="menu_btn" @click="menuToggle" >
      <span :class="[menuOpen ? 'menu_default_active' : '', 'menu_default']"></span>
      <span :class="[menuOpen ? '' : 'menu_focus_active', 'menu_focus']"></span>
    </button> 
</template>

<script>
export default {
  name: "MenuButton",
  data() {
    return {
      menuOpen: false,
    };
  },
  emits: ["menu-open", "menu-close"],
  methods: {
    menuToggle() {
      this.menuOpen = !this.menuOpen;
      if (this.menuOpen) {
        this.$emit("menu-open");
      } else {
        this.$emit("menu-close");
      }
    },
  },
};
</script>

<style scoped>
.menu_btn {
  position: relative;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  width: 2.6vw;
  height: 2.6vw;
  background-color: transparent;
  padding: 0;
  border: none;
  cursor: pointer;
}

.menu_default, .menu_focus {
  position: absolute;
  display: block;
  z-index: 1;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  width: 20px;
  height: 7px;
  transition: .15s ease-in-out;
}

.menu_default:hover {
  height: 15px;
  transition: .15s ease-in-out;
}

.menu_default::before, .menu_default::after {
  position: absolute;
  left: 0;
  display: block;
  content: "";
  width: 100%;
  height: 2px;
  background-color: var(--font-color);
  transition: 0.2s;
}

.menu_default::after {
  bottom: 0;
}
.menu_default::before {
  top: 0;
}

.menu_default_active {
  z-index: 0;
}

.menu_default_active::before, .menu_default_active::after {
  opacity: 0;
  transition: 0.2s;
}

.menu_default_active::before {
  transform: translate(-1vw, 0);
}
.menu_default_active::after {
  transform: translate(1vw, 0);
}

.menu_focus {
  width: 20px;
  height: 20px;
}

.menu_focus:hover {
  transform: rotate(90deg);
  transition: .15s ease-in-out;
}

.menu_focus::before, .menu_focus::after{
  display: block;
  position: absolute;
  left: 0;
  top: 50%;
  content: "";
  width: 100%;
  height: 2px;
  background-color: black;
  transform: rotate(45deg);
  transition: 0.15s ease-in-out;
}

.menu_focus::after{
  transform: rotate(-45deg);
}

.menu_focus_active {
  z-index: 0;
}

.menu_focus_active::after, .menu_focus_active::before {
  opacity: 0;
  transition: 0.15s ease-in-out;
}

.menu_focus_active::before {
  transform: translate(-1vw, -1vw) rotate(45deg);
}

.menu_focus_active::after {
  transform: translate(1vw, -1vw) rotate(-45deg);
}
</style>