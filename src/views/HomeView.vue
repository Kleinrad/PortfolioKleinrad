<template>
  <div class="wrapper" :style="{'position': 'fixed'}">
    <MenuFull v-if="showMenu"></MenuFull>
    <div class="scroll_wrapper" :style="{'position': 'absolute', 'top': Math.min(-wrapperY,0) + 'px'}">
      <CursorDot :dynamic="dynamic_cursor" :pos="{x:8, y:103}" v-if="show_cursor" :abs_top_offset="Math.min(-wrapperY,0)" />
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
          <HomeBanner id="banner"></HomeBanner>
          <ProjectsOverview id="projects" :lineLength="pLine" :dotDistance="project_Dot_pos" :active-video="item_active"></ProjectsOverview>
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
      wrapperY: 0,
      scrollDirection: 0,
      scrollSpeed: 1,

      currMenuPoint: -1,
      pLine: -1,
      project_Dot_pos: 0,
      item_active: -1,
      height: 0,

      scrollEvents: [],

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
    handleScroll(scrollDelta) {
      this.checkEvent()
      if(scrollDelta == undefined) return;
      this.scrollCounter = Math.min(Math.max(this.scrollCounter + scrollDelta*this.scrollSpeed, 0),
                            Math.round(this.height/100)*100);
      this.wrapper_top = this.scrollCounter;
      console.log(this.scrollSpeed);
      this.scrollSpeed = 1;
      return;
    },
    checkEvent() {
      this.scrollEvents.every((event, i) => {
        if(this.scrollCounter == event.start || event.permenent){
          
          if((this.scrollDirection == 1 && event.on_down_scroll && !event.permenent)
            || this.scrollCounter > event.start){
            event.on_down_scroll();
          }
          if((this.scrollDirection == -1 && event.on_up_scroll && !event.permenent)
            || this.scrollCounter < event.start){
            event.on_up_scroll();
          }
        }
        if(event.length && this.scrollCounter >= event.start && this.scrollCounter <= event.start + event.length){
          if(event.on_scroll && 
            (this.scrollDirection == -1 && this.scrollCounter > event.start ||
             this.scrollDirection == 1 && this.scrollCounter < event.start + event.length) ){
            event.on_scroll();
          }
          if(event.length + event.start == this.scrollCounter && event.on_scroll_end){
            event.on_scroll_end();
          }
        }
        return true;
      });
    },
    scrollIntercept(e) {
      let scrollDelta = 0;
      if (this.touchStart != -1) {
        this.scrollDirection = this.touchStart - e.touches[0].pageY > 0 ? 1 : -1;
        scrollDelta = Math.round((this.touchStart - e.touches[0].pageY)*2);
        this.touchStart = e.touches[0].pageY;
      }else{
        this.scrollDirection = e.deltaY > 0 ? 1 : -1;
        scrollDelta = Math.round(e.deltaY);
      }
      this.handleScroll(scrollDelta);
    },
    startTouchScroll(e) {
      this.touchStart = e.touches[0].pageY;
    },
    endTouchScroll(e) {
      this.touchStart = -1;
      this.scrollDirection = 0;
    },
  },
  mounted() {
    this.scrollEvents =  [
        {start: Math.round(document.getElementById("banner").scrollHeight/100)*100-100, 
         permenent: true,
          on_down_scroll: ()=>{
            this.currMenuPoint = 0;
            this.showHeaderBase = false;
            this.dynamic_cursor = false;
            document.documentElement.style.setProperty("--accent-color", this.colors.accentColor[1]);
            document.documentElement.style.setProperty("--secondary-color", this.colors.secondaryColor[1]);
            document.documentElement.style.setProperty("--background-color", this.colors.backgroundColor[1]);
            document.documentElement.style.setProperty("--font-color", this.colors.fontColor[1]);
          }, 
          on_up_scroll: ()=>{
            this.item_active = -1;
            this.dynamic_cursor = true;
            this.showHeaderBase = true;
            this.currMenuPoint = -1;
            this.pLine = 0;
            document.documentElement.style.setProperty("--accent-color", this.colors.accentColor[0]);
            document.documentElement.style.setProperty("--secondary-color", this.colors.secondaryColor[0]);
            document.documentElement.style.setProperty("--background-color", this.colors.backgroundColor[0]);
            document.documentElement.style.setProperty("--font-color", this.colors.fontColor[0]);
          }
          },
        {start: Math.round(document.getElementById("banner").scrollHeight/100)*100,
         length: Math.round(document.getElementById("projects").scrollHeight/100)*100, 
          on_down_scroll: ()=>{
            this.pLine = 100;
            this.item_active = -1;
            this.show_cursor = false;
          },
          on_up_scroll: ()=>{
            this.show_cursor = true;
            this.item_active = -1;
          },
          on_scroll: ()=>{
            this.project_Dot_pos = (this.scrollCounter - (this.scrollEvents[1].start)) / this.scrollEvents[1].length * 100;
            this.scrollSpeed = 0.2;
          },
        },
      ],

    window.addEventListener("scroll", this.handleScroll);
    window.addEventListener("wheel", this.scrollIntercept, { passive: false });
    //phone scroll
    window.addEventListener("touchmove", this.scrollIntercept, { passive: false });
    window.addEventListener("touchstart", this.startTouchScroll, { passive: false });
    window.addEventListener("touchend", this.endTouchScroll, { passive: false });

    this.height = document.getElementsByClassName("scroll_wrapper")[0].scrollHeight;

    for(let i=0; i<this.scrollEvents.length; i++){
      this.scrollEvents[i]['scrollDistance'] = 0;
    }

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
