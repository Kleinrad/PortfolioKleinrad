<template>
  <div class="wrapper" :style="{'position': 'fixed'}">
    <MenuFull v-if="showMenu"></MenuFull>
    <div class="scroll_wrapper" :style="{'position': 'absolute', 'top': Math.min(-wrapperY,0) + 'px'}">
      <CursorDot :dynamic="dynamic_cursor" :pos="{x:8, y:103}" v-if="show_cursor" :abs_top_offset="Math.min(-wrapperY,0)" />
      <HeaderBar 
      @menu-close="showMenu=false" 
      @menu-open="showMenu=true"
      @scroll-projects="scrollTo('projects')"
      @scroll-about="scrollTo('about')"
      v-if="showHeaderBase"
      ></HeaderBar>
      <HeaderSideBar 
      :menu_point="currMenuPoint"
      @scroll-projects="scrollTo('projects')"
      @scroll-about="scrollTo('about')"
      v-if="!showHeaderBase"
      ></HeaderSideBar>
      <Transition name="fade">
        <div class="mainPage" v-if="!showMenu">
          <HomeBanner id="banner"></HomeBanner>
          <ProjectsOverview id="projects" :lineLength="pLine" :dark="darkMode" :dotDistance="project_Dot_pos" :active-video="item_active" :screenType="screenType"></ProjectsOverview>
          <AboutMe id="about" :visible="showAbout" :screenType="screenType" :scrollCount="aboutCount"></AboutMe>
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
import AboutMe from "@/components/AboutMe.vue";
import { useMediaQuery } from '@vueuse/core'

import CursorDot from "@/components/CursorDot.vue";

export default {
  name: "HomeView",
  data() {
    return {
      showMenu: false,
      showHeaderBase: true,
      dynamic_cursor: true,
      show_cursor: true,
      darkMode: false,
      screenType: 0, //0 = mobile, 1 = tablet, 2 = desktop

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
      showAbout: false,

      touchStart: -1,
      aboutCount: 0,
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
    AboutMe,
  },
  methods: {
    handleScroll(scrollDelta) {
      this.checkEvent()
      if(scrollDelta == undefined) return;
      this.scrollInput = Math.min(Math.max(this.scrollInput + scrollDelta*this.scrollSpeed, 0), this.height-this.viewHeight);
      this.scrollCounter = Math.floor(this.scrollInput/(100*this.scrollSpeed))*(100*this.scrollSpeed);
      this.wrapper_top = this.scrollInput;
      this.scrollSpeed = 0.5;
      this.updateAboutCount();
      return;
    },
    checkEvent(jump = false) {
      this.scrollEvents.every((event, i) => {
        let elementBounds = document.getElementById(event.id).getBoundingClientRect();
        let currScore = Math.floor((elementBounds.height+event.top) / (this.scrollSpeed*100))*(this.scrollSpeed*100)-this.scrollCounter;
        let triggerPoint = Math.floor((elementBounds.height*(1-event.trigger)) / (this.scrollSpeed*100))*(this.scrollSpeed*100);
        if(currScore == triggerPoint || event.permenent || (jump && currScore+jump >= triggerPoint && currScore <= triggerPoint)){
          
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
          if(currScore <= 100 && event.on_scroll_end){
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
    scrollTo(id){
      let elementBounds = document.getElementById(id).getBoundingClientRect();
      this.scrollInput = Math.min(Math.max(this.scrollInput + elementBounds.top, 0), this.height);
      this.scrollCounter = Math.floor(this.scrollInput/(100*this.scrollSpeed))*(100*this.scrollSpeed);
      this.wrapper_top = this.scrollInput;
      this.checkEvent(elementBounds.top);
      this.scrollSpeed = 0.5;
      return;
    },
    updateAboutCount(){
      let elementBounds = document.getElementById("about").getBoundingClientRect();
      let top = this.scrollEvents[2].top;
      if(this.scrollCounter - top < 0){
        this.aboutCount = 0;
      }else if(this.scrollCounter - (top + (elementBounds.height-this.viewHeight)) > 0){
        this.aboutCount = 100;
      }else{
        this.aboutCount = Math.floor((this.scrollCounter - top)/(elementBounds.height-this.viewHeight)*100);
      }
    },
  },
  mounted() {
    this.scrollEvents =  [
        {id: "banner", 
         trigger: 0.4,
         permenent: true,
          on_down_scroll: ()=>{
            if (this.currMenuPoint == -1){
              this.currMenuPoint = 0;
            }
            this.showHeaderBase = false;
            this.dynamic_cursor = false;
            this.darkMode = true;
            document.documentElement.style.setProperty("--accent-color", this.colors.accentColor[1]);
            document.documentElement.style.setProperty("--secondary-color", this.colors.secondaryColor[1]);
            document.documentElement.style.setProperty("--background-color", this.colors.backgroundColor[1]);
            document.documentElement.style.setProperty("--font-color", this.colors.fontColor[1]);
          }, 
          on_up_scroll: ()=>{
            this.item_active = -1;
            this.dynamic_cursor = true;
            this.showHeaderBase = true;
            this.darkMode = false;
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
         downScroll: false,
          on_down_scroll: ()=>{
            this.scrollEvents[1].downScroll = true;
            this.pLine = 100;
            this.item_active = -1;
            this.show_cursor = false;
          },
          on_up_scroll: ()=>{
            this.scrollEvents[1].downScroll = false;
            this.show_cursor = true;
            this.item_active = -1;
          },
          on_scroll: (obj)=>{
            if(!this.scrollEvents[1].downScroll) this.scrollEvents[1].on_down_scroll();
            this.currMenuPoint = 0;
            let offCenter = Math.max(((obj.bounds.height-(obj.currScore))/obj.bounds.height),0) * (this.viewHeight/2);
            this.project_Dot_pos = Math.min(100,Math.max((obj.triggerPoint-(obj.currScore - offCenter))/obj.triggerPoint*100, 0.2));
            this.scrollSpeed = 0.4;
          },
          on_scroll_end: ()=>{
            this.currMenuPoint=1;
          }
        },
        {id: "about",
         trigger: -0.2,
         downScroll: false,
          on_down_scroll: ()=>{
            this.scrollEvents[2].downScroll = true;
            this.item_active = -1;
            this.showAbout = true;
          },
          on_up_scroll: ()=>{
            this.scrollEvents[2].downScroll = false;
            this.showAbout = false;
          },
        },
      ],

    window.addEventListener("scroll", this.scrollIntercept, { passive: false });
    window.addEventListener("wheel", this.scrollIntercept, { passive: false });
    //phone scroll
    window.addEventListener("touchmove", this.scrollIntercept, { passive: false });
    window.addEventListener("touchstart", this.startTouchScroll, { passive: false });
    window.addEventListener("touchend", this.endTouchScroll, { passive: false });


    if (useMediaQuery('(max-width: 768px)').value) {
      this.screenType = 0;
    }else if (useMediaQuery('(max-width: 1050px)').value) {
      this.screenType = 1;
    }else{
      this.screenType = 2;
    }

    this.handleScroll();
    setInterval(() => {
      this.wrapperY += (this.wrapper_top - this.wrapperY) * 0.1;
    }, 1000 / 60);
    
    this.wrapperY = 1

    setTimeout(() => {
      if(!this.scrollEvents[0].top){
        for(let i=0; i<this.scrollEvents.length; i++){
          this.scrollEvents[i]['top'] = document.getElementById(this.scrollEvents[i].id).getBoundingClientRect().top;
        }
      }
      this.height = document.getElementsByClassName("scroll_wrapper")[0].scrollHeight;
      this.viewHeight = window.innerHeight;
      this.scrollTo('about')
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
