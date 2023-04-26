<template>
    <div class="menu_wrapper" @mouseleave="closeMenu" @click="closeMenu">
        <div class="menu_bg"></div>
        <span class="menu_point" v-for="(point, index) in menu_points" :key="index"
        @click="scrollTo(point)"
        :style="{'opacity' : menu_point == index ? 1 : 0.3, 'margin-left' : menu_point == index ? '4vw' : '3vw'}">
            {{point}}
        </span>
        <div class="menu_icons">
            <a href="https://www.linkedin.com/in/fabian-kleinrad-27892522b/">
                <img id="linkedIn_menu" src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png">
            </a>
            <a href="https://www.linkedin.com/in/fabian-kleinrad-27892522b/">
                <img id="email_menu" src="https://upload.wikimedia.org/wikipedia/commons/8/8e/Android_Emoji_2709.svg">
            </a>
        </div>
    </div>
</template>

<script>
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
}
</script>

<style scoped>
.menu_wrapper {
    position: fixed;
    box-sizing: border-box;
    top: 0;
    right: 0;
    z-index: 10;
    width: 20vw;
    height: 100svh;
    box-shadow: 0px -4px 20px 0px rgba(34, 34, 34, 0.25);
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-direction: column;
    transform: translateX(16vw);
    animation: slide-In 0.6s ease-in-out forwards;

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
    transition: 0.5s ease-in-out;
    cursor: pointer;
}

.menu_icons {
    position: absolute;
    bottom: 1%;
    height: 3vw;
    width: 100%;
    filter: invert(1) saturate(0) brightness(1.4);
    display: flex;
    box-sizing: border-box;
    padding-left: 3vw;
    justify-content: left;
}

.menu_icons a {
    display: flex;
    align-items: center;
    opacity: 0.7;
    transition: opacity 0.5s ease-in-out;
}

.menu_icons a:hover {
    opacity: 1;
}

#linkedIn_menu {
    height: 65%;
    aspect-ratio: 1/1;
}

#email_menu {
    height: 100%;
    aspect-ratio: 2/1;
}


@keyframes slide-In {
    0% {
        transform: translateX(80%);
    }
    100% {
        transform: translateX(0%);
    }
}
</style>