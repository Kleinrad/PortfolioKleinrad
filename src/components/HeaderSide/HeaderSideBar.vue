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
                <img id="email" src="https://upload.wikimedia.org/wikipedia/commons/8/8e/Android_Emoji_2709.svg">
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

.header_icons {
    position: absolute;
    left: 2%;
    margin-bottom: -0.5vw;
    width: 2.5vw;
    display: flex;
    flex-direction: column;
    transform: rotate(90deg);
    filter: invert(1) saturate(0) brightness(1.4);
}

.header_icons img{
    aspect-ratio: 1/1;
    opacity: 0.3;
    transition: opacity 0.5s;
}

#linkedIn {
    width: 70%;
    aspect-ratio: 1/1;
}

#email {
    width: 100%;
    aspect-ratio: 1/1;
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
        transform: none;
        box-shadow: -4px 0px 20px 0px rgba(34, 34, 34, 0.25);
    }

    .header_icons {
        left: auto;
        right: 0%;
        height: 13vw;
        width: fit-content;
        flex-direction: row;
        transform: rotate(0deg);
    }

    .header_icons a{
        display: flex;
        padding: 1vw;
        align-items: center;
    }

    #linkedIn {
        height: 58%;
        width: auto;
        aspect-ratio: 1/1;
    }

    #email {
        height: 100%;
        width: auto;
        aspect-ratio: 1/1;
    }
}
</style>