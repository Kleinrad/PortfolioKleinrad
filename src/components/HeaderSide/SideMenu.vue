<template>
    <div class="menu_wrapper" @mouseleave="closeMenu" @click="closeMenu">
        <div class="menu_bg"></div>
        <span class="menu_point" v-for="(point, index) in menu_points" :key="index"
        @click="scrollTo(point)"
        :style="{'opacity' : menu_point == index ? 1 : 0.3, 'margin-left' : menu_point == index ? '4vw' : '3vw'}">
            {{point}}
        </span>
        <SocialIcons :dark="true"/>
    </div>
</template>

<script>
import SocialIcons from "@/components/SocialIcons.vue"

export default {
    data() {
        return {
        }
    },
    props: {
        "menu_points": Array,
        "menu_point": Number,
    },
    emits: ["closeMenu", "scroll-projects", "scroll-about"],
    methods: {
        closeMenu() {
            this.$emit("closeMenu");
        },
        scrollTo(id) {
            this.$emit("scroll-" + id);
        }
    },
    components: {
        SocialIcons,
    },
}
</script>

<style scoped>
/*
* Prefixed by https://autoprefixer.github.io
* PostCSS: v8.4.14,
* Autoprefixer: v10.4.7
* Browsers: last 4 version
*/

.menu_wrapper {
    position: fixed;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    top: 0;
    right: 0;
    z-index: 10;
    width: 20vw;
    height: 100svh;
    -webkit-box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
            box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: start;
        -ms-flex-align: start;
            align-items: flex-start;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-transform: translateX(16vw);
        -ms-transform: translateX(16vw);
            transform: translateX(16vw);
    -webkit-animation: slide-In 0.6s cubic-bezier(.56,-0.01,.76,.98) forwards;
            animation: slide-In 0.6s cubic-bezier(.56,-0.01,.76,.98) forwards;

    font-size: 2vw;
}

.menu_bg {
    position: absolute;
    top: 0;
    left: 0;
    z-index:-1;
    width: 100%;
    height: 100%;
    background-color: var(--background-color);
    opacity: 0.3;
}

.menu_wrapper > span {
    opacity: 0.3;
    margin-bottom: 1vw;
    -webkit-transition: 0.5s ease-in-out;
    -o-transition: 0.5s ease-in-out;
    transition: 0.5s ease-in-out;
    cursor: pointer;
}




@-webkit-keyframes slide-In {
    0% {
        -webkit-transform: translateX(100%);
                transform: translateX(100%);
    }
    100% {
        -webkit-transform: translateX(0%);
                transform: translateX(0%);
    }
}


@keyframes slide-In {
    0% {
        -webkit-transform: translateX(100%);
                transform: translateX(100%);
    }
    100% {
        -webkit-transform: translateX(0%);
                transform: translateX(0%);
    }
}

@media screen and (max-width: 1050px) {
    .menu_wrapper {
        width: 20vw;
        height: 100svh;
        -webkit-transform: translateX(20vw);
            -ms-transform: translateX(20vw);
                transform: translateX(20vw);
    }

    .menu_wrapper > span {
        font-size: 3vw;
    }
}

@media screen and (max-width: 768px) {
    .menu_wrapper {
        width: 100vw;
        height: 100svh;
        -webkit-transform: translateY(100vw);
            -ms-transform: translateY(100vw);
                transform: translateY(100vw);
        pointer-events: all;
        -webkit-box-align: center;
            -ms-flex-align: center;
                align-items: center;
    }

    .menu_bg{
        opacity: 0.95;
    }
    .menu_wrapper > span {
        font-size: 15vw;
    }

    @-webkit-keyframes slide-In {
        0% {
            -webkit-transform: translateY(-100%);
                    transform: translateY(-100%);
        }
        100% {
            -webkit-transform: translateY(0%);
                    transform: translateY(0%);
        }
    }

    @keyframes slide-In {
        0% {
            -webkit-transform: translateY(-100%);
                    transform: translateY(-100%);
        }
        100% {
            -webkit-transform: translateY(0%);
                    transform: translateY(0%);
        }
    }
}
</style>