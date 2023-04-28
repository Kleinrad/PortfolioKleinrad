<template>
    <div class="header_wrapper" @mouseenter="isHovered = true" @mouseleave="isHovered = false" v-if="!isOpen" @click="isOpen = true; isHovered = false">
        <div class="menu_point" v-for="point, i in menu_points"  :key="i" :style="{'display': i == menu_point ? 'flex' : 'none'}">
            <MenuPoint :point="point" :isHovered="isHovered"></MenuPoint>
        </div>
        <div class="header_icons">
            <a href="https://www.linkedin.com/in/fabian-kleinrad-27892522b/">
                <img id="linkedIn" src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png">
            </a>
            <a href="mailto:fabiankleinrad.fk@gmail.com">
                <img id="email" src="https://upload.wikimedia.org/wikipedia/commons/7/7f/OOjs_UI_icon_message.svg">
            </a>
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
    -webkit-transition: 0.5s;
    -o-transition: 0.5s;
    transition: 0.5s;
}

.sideMenu-enter, .sideMenu-leave-to {
    -webkit-transform: translateX(-100%);
        -ms-transform: translateX(-100%);
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
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    -webkit-transform: rotate(-90deg) translateY(-100%);
        -ms-transform: rotate(-90deg) translateY(-100%);
            transform: rotate(-90deg) translateY(-100%);
    -webkit-transform-origin: top right;
        -ms-transform-origin: top right;
            transform-origin: top right;
    padding-bottom: 0.5vw;
    -webkit-box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
            box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
    -webkit-transition: 1s, var(--bg-transition);
    -o-transition: 1s, var(--bg-transition);
    transition: 1s, var(--bg-transition);

    font-size: 2vw;
    font-weight: 400;

    &:hover { 
        -webkit-box-shadow: 0px -4px 20px 0px rgba(92, 92, 92, 0.45); 
                box-shadow: 0px -4px 20px 0px rgba(92, 92, 92, 0.45);
    }
}

.header_icons {
    position: absolute;
    left: 4%;
    margin-bottom: -0.5vw;
    width: 2vw;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-transform: rotate(90deg);
        -ms-transform: rotate(90deg);
            transform: rotate(90deg);
    -webkit-filter: invert(1) saturate(0) brightness(1.4);
            filter: invert(1) saturate(0) brightness(1.4);
}

.header_icons img{
    aspect-ratio: 1/1;
    opacity: 0.3;
    width: 100%;
    -webkit-transition: opacity 0.5s;
    -o-transition: opacity 0.5s;
    transition: opacity 0.5s;
}

.header_icons img:hover {
    opacity: 1;
}

@media screen and (max-width: 1050px) {
    .header_wrapper {
        height: 6vw;
    }

    .header_icons {
        width: 3.5vw;
    }
}

@media screen and (max-width: 768px) {
    .header_wrapper {
        height: 15vw;
        width: 100vw;
        -webkit-transform: none;
            -ms-transform: none;
                transform: none;
        -webkit-box-shadow: -4px 0px 20px 0px rgba(34, 34, 34, 0.25);
                box-shadow: -4px 0px 20px 0px rgba(34, 34, 34, 0.25);
    }

    .header_icons {
        left: auto;
        right: 2%;
        height: 11vw;
        width: -webkit-fit-content;
        width: -moz-fit-content;
        width: fit-content;
        -webkit-box-orient: horizontal;
        -webkit-box-direction: normal;
            -ms-flex-direction: row;
                flex-direction: row;
        -webkit-transform: rotate(0deg);
            -ms-transform: rotate(0deg);
                transform: rotate(0deg);
    }

    .header_icons a{
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        padding: 1vw;
        -webkit-box-align: center;
            -ms-flex-align: center;
                align-items: center;
    }

    .header_icons img {
        height: 100%;
        margin: 0 1vw;
        width: auto;
    }
}
</style>