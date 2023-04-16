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

      scrollInput: 0, //raw scroll distance
      scrollCounter: 0, //scroll distance in 100px * scrollSpeed increments
      wrapper_top: 0, //positon of wrapper to transition to
      wrapperY: 0,  //wrapper top offset
      scrollDirection: 0, //1 = down, -1 = up, 0 = no scroll
      scrollSpeed: 0.5, //scroll speed multiplier

      currMenuPoint: -1,
      pLine: -1,
      project_Dot_pos: 0,
      item_active: -1,
      height: 0,
      viewHeight: 0,

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
      this.scrollInput = Math.min(Math.max(this.scrollInput + scrollDelta*this.scrollSpeed, 0), this.height);
      this.scrollCounter = Math.floor(this.scrollInput/(100*this.scrollSpeed))*(100*this.scrollSpeed);
      this.wrapper_top = this.scrollInput;
      this.scrollSpeed = 0.5;
      return;
    },
    checkEvent() {
      this.scrollEvents.every((event, i) => {
        let elementBounds = document.getElementById(event.id).getBoundingClientRect();
        let currScore = Math.floor((elementBounds.height+event.top) / (this.scrollSpeed*100))*(this.scrollSpeed*100)-this.scrollCounter;
        let triggerPoint = Math.floor((elementBounds.height*(1-event.trigger)) / (this.scrollSpeed*100))*(this.scrollSpeed*100);
        if(currScore == triggerPoint || event.permenent){
          
          if((this.scrollDirection == 1 && event.on_down_scroll && !event.permenent)
            || currScore < triggerPoint){
            event.on_down_scroll();
          }
          if((this.scrollDirection == -1 && event.on_up_scroll && !event.permenent)
            || currScore > triggerPoint){
            event.on_up_scroll();
          }
        }
        if(event.continuous && currScore <= triggerPoint && elementBounds.bottom >= 0){
          if(event.on_scroll){
            event.on_scroll({bounds: elementBounds, triggerPoint: triggerPoint, currScore: currScore});
          }
          if(currScore == 0 && event.on_scroll_end){
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
        {id: "banner", 
         trigger: 0.4,
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
        {id: "projects",
         trigger: -0.1,
         continuous: true,
          on_down_scroll: ()=>{
            this.pLine = 100;
            this.item_active = -1;
            this.show_cursor = false;
          },
          on_up_scroll: ()=>{
            this.show_cursor = true;
            this.item_active = -1;
          },
          on_scroll: (obj)=>{
            let offCenter = Math.max(((obj.bounds.height-(obj.currScore))/obj.bounds.height),0) * (this.viewHeight/2);
            this.project_Dot_pos = Math.min(100,Math.max((obj.triggerPoint-(obj.currScore - offCenter))/obj.triggerPoint*100, 0.2));
            this.scrollSpeed = 0.2;
          },
        },
      ],

    window.addEventListener("scroll", this.scrollIntercept, { passive: false });
    window.addEventListener("wheel", this.scrollIntercept, { passive: false });
    //phone scroll
    window.addEventListener("touchmove", this.scrollIntercept, { passive: false });
    window.addEventListener("touchstart", this.startTouchScroll, { passive: false });
    window.addEventListener("touchend", this.endTouchScroll, { passive: false });

    this.height = document.getElementsByClassName("scroll_wrapper")[0].scrollHeight;
    this.viewHeight = window.innerHeight;

    for(let i=0; i<this.scrollEvents.length; i++){
      this.scrollEvents[i]['top'] = document.getElementById(this.scrollEvents[i].id).getBoundingClientRect().top;
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
