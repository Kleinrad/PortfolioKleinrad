<template>
    <div class="header_wrapper" @mouseenter="isHovered = true" @mouseleave="isHovered = false" v-if="!isOpen" @click="isOpen = true; isHovered = false">
        <div class="menu_point" v-for="point, i in menu_points"  :key="i" :style="{'display': i == menu_point ? 'flex' : 'none'}">
            <MenuPoint :point="point" :isHovered="isHovered"></MenuPoint>
        </div>
    </div>
    <Transition name="sideMenu">
        <SideMenu v-if="isOpen" 
        :menu_points="menu_points" :menu_point="menu_point" 
        @closeMenu="isOpen = false"
        @scroll-projects="scrollProjects"
        @scroll-about="scrollAbout"
        ></SideMenu>
    </Transition>
</template>

<script>
import MenuPoint from '@/components/HeaderSide/MenuPoint.vue'
import SideMenu from '@/components/HeaderSide/SideMenu.vue'

export default {
    data() {
        return {
            menu_points: ["projects", "about"],
            isHovered: false,
            isOpen: false,
        }
    },
    props: {
        menu_point: Number,
    },
    components: {
        MenuPoint,
        SideMenu,
    },
    emits: ["scroll-projects", "scroll-about"],
    methods: {
        scrollProjects() {
            this.$emit("scroll-projects");
        },
        scrollAbout() {
            this.$emit("scroll-about");
        },
    },
}
</script>

<style lang="scss">
.sideMenu-enter-active, .sideMenu-leave-active {
    transition: 0.5s;
}

.sideMenu-enter, .sideMenu-leave-to {
    transform: translateX(-100%);
    opacity: 0;
}

.header_wrapper {
    position: fixed;
    top: 0;
    right: 0;
    width: 100svh;
    height: 4vw;
    background-color: var(--background-color);
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    transform: rotate(-90deg) translateY(-100%);
    transform-origin: top right;
    padding-bottom: 0.5vw;
    box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
    transition: 1s, var(--bg-transition);

    font-size: 2vw;
    font-weight: 400;

    &:hover { 
        box-shadow: 0px -4px 20px 0px rgba(92, 92, 92, 0.45);
    }
}
</style>