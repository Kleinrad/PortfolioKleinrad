<template>
  <div class="wrapper" :style="{'position': 'fixed'}">
    <MenuFull v-if="showMenu"></MenuFull>
    <div class="scroll_wrapper" :style="{'position': 'absolute', 'top': Math.min(-wrapperY,0) + 'px'}">
      <CursorDot :dynamic="dynamic_cursor" :pos="{x:110, y:957.8}" v-if="show_cursor" :abs_top_offset="Math.min(-wrapperY,0)" />
      <HeaderBar 
      @menu-close="showMenu=false" 
      @menu-open="showMenu=true"
      v-if="showHeaderBase"
      ></HeaderBar>
      <HeaderSideBar 
      :menu_point="currMenuPoint"
      v-if="!showHeaderBase"
      ></HeaderSideBar>
      <Transition name="fade">
        <div class="mainPage" v-if="!showMenu">
          <HomeBanner></HomeBanner>
          <ProjectsOverview :lineLength="pLine" :dotDistance="project_Dot_pos" :active-video="item_active"></ProjectsOverview>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script>
import HeaderBar from "@/components/HeaderBase/HeaderBar.vue";
import HeaderSideBar from "@/components/HeaderSide/HeaderSideBar.vue";
import MenuFull from "@/components/MenuFull.vue";
import HomeBanner from "@/components/Home/HomeBanner.vue";
import ProjectsOverview from "@/components/Home/Projects/ProjectsOverview.vue";

import CursorDot from "@/components/CursorDot.vue";

export default {
  name: "HomeView",
  data() {
    return {
      showMenu: false,
      showHeaderBase: true,
      dynamic_cursor: true,
      show_cursor: true,
      scrollCounter: 0,
      wrapper_top: 0,
      currMenuPoint: -1,
      pLine: -1,
      project_Dot_pos: 0,
      item_active: -1,

      wrapperY: 0,
      touchStart: -1,
      colors: {
        accentColor: ["#4eeed9", "#A1CEB3"],
        secondaryColor: ["#97b1bb", "#1B2626"],
        backgroundColor: ["#fff", "#080E0E"],
        fontColor: ["#000", "#EFFBF3"],
      },
    };
  },
  components: {
    HeaderBar,
    MenuFull,
    HomeBanner,
    ProjectsOverview,
    HeaderSideBar,
    CursorDot,
  },
  methods: {
    handleScroll() {
      if(this.scrollCounter > 800){
        this.show_cursor = false;
        this.pLine = 100;
        this.item_active = 0;
        if(this.scrollCounter > 900){
          this.project_Dot_pos = (this.scrollCounter - 900) / 10000 * 100;
        }
        return;
      }

      if (!this.isDark && this.scrollCounter > 550) {
        this.isDark = true;
        this.currMenuPoint = 0;
        this.showHeaderBase = false;
        this.dynamic_cursor = false;
        document.documentElement.style.setProperty("--accent-color", this.colors.accentColor[1]);
        document.documentElement.style.setProperty("--secondary-color", this.colors.secondaryColor[1]);
        document.documentElement.style.setProperty("--background-color", this.colors.backgroundColor[1]);
        document.documentElement.style.setProperty("--font-color", this.colors.fontColor[1]);
      }else if (this.isDark && this.scrollCounter < 450) {
        this.isDark = false;
        this.dynamic_cursor = true;
        this.show_cursor = true;
        this.item_active = -1;
        this.showHeaderBase = true;
        this.pLine = 0;
        this.currMenuPoint = -1;
        document.documentElement.style.setProperty("--accent-color", this.colors.accentColor[0]);
        document.documentElement.style.setProperty("--secondary-color", this.colors.secondaryColor[0]);
        document.documentElement.style.setProperty("--background-color", this.colors.backgroundColor[0]);
        document.documentElement.style.setProperty("--font-color", this.colors.fontColor[0]);
      }
      this.wrapper_top = this.scrollCounter;
    },
    scrollIntercept(e) {
      if (this.touchStart != -1) {
        this.scrollCounter += Math.round((this.touchStart - e.touches[0].pageY)* 2);
        this.touchStart = e.touches[0].pageY;
      }else{
        this.scrollCounter = Math.max(e.deltaY + this.scrollCounter, 0);
      }
      this.handleScroll();
    },
    startTouchScroll(e) {
      this.touchStart = e.touches[0].pageY;
    },
    endTouchScroll(e) {
      this.touchStart = -1;
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    window.addEventListener("wheel", this.scrollIntercept, { passive: false });
    //phone scroll
    window.addEventListener("touchmove", this.scrollIntercept, { passive: false });
    window.addEventListener("touchstart", this.startTouchScroll, { passive: false });
    window.addEventListener("touchend", this.endTouchScroll, { passive: false });
    this.handleScroll();
    setInterval(() => {
      this.wrapperY += (this.wrapper_top - this.wrapperY) * 0.1;
    }, 1000 / 60);
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

.wrapper {
  position: relative;
  width: 100%;
  height: 100svh;
  overflow: hidden;
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
