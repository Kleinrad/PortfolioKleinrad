<template>
    <div class="menu_wrapper">
        <div class="menu_inner">
            <span @click="scrollProjects">projects</span>
            <span @click="scrollAbout">about</span>
        </div>
        <div class="menu_footer">
            <span>Say Hello (:</span>
            <div class="socials"><SocialIcons/></div>
        </div>
    </div>
</template>

<script>
import SocialIcons from '@/components/SocialIcons.vue'

export default {
    name: "MenuFull",
    data() {
        return {
        }
    },
    props: {
    },
    mounted() {
    },
    emits: ['scroll-projects', 'scroll-about', 'closemenu'],
    methods: {
        scrollProjects() {
            this.$emit("scroll-projects");
            this.$emit("closemenu");
        },
        scrollAbout() {
            this.$emit("scroll-about");
            this.$emit("closemenu");
        },
    },
    components: {
        SocialIcons,
    },
}
</script>

<style scoped>
.menu_wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100svh;
    background-color: #fff;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: start;
        -ms-flex-align: start;
            align-items: flex-start;
    -webkit-box-pack: space;
        -ms-flex-pack: space;
            justify-content: space-between;
    padding-left: 5vw;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    -webkit-animation: fadeIn .5s forwards;
            animation: fadeIn .5s forwards;
}

.menu_inner {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    font-size: 4em;
    font-weight: 1000;
    height: 50vh;
    text-align: left;
    margin: auto;
    margin-left: 0;
}

.menu_inner > span {
    opacity: 0.3;
    height: 95px;
    -webkit-animation: slide-together .15s forwards;
            animation: slide-together .15s forwards;
}

.menu_inner > span:nth-child(1) { -webkit-animation-delay: 0.1s; animation-delay: 0.1s; }
.menu_inner > span:nth-child(2) { -webkit-animation-delay: 0.15s; animation-delay: 0.15s; }
.menu_inner > span:nth-child(3) { -webkit-animation-delay: 0.2s; animation-delay: 0.2s; }


.menu_inner > span {
    cursor: pointer;
    -webkit-transition: 0.3s ease-in-out;
    -o-transition: 0.3s ease-in-out;
    transition: 0.3s ease-in-out;
}

.menu_inner > span:hover {
    margin: 0 1vw;
    color: var(--accent-color);
    -webkit-transition: color 0.3 cubic-bezier(.39,.575,.565,1), -webkit-transform 0.35s;
    transition: color 0.3 cubic-bezier(.39,.575,.565,1), -webkit-transform 0.35s;
    -o-transition: transform 0.35s, color 0.3 cubic-bezier(.39,.575,.565,1);
    transition: transform 0.35s, color 0.3 cubic-bezier(.39,.575,.565,1);
    transition: transform 0.35s, color 0.3 cubic-bezier(.39,.575,.565,1), -webkit-transform 0.35s;
}

.menu_footer {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    box-sizing: border-box;
    padding: 4vw;
    width: 100%;
    margin-bottom: 1svh;
}

@-webkit-keyframes slide-together {
    100% {
        height: 80px;
        opacity: 1;
    }
}

@keyframes slide-together {
    100% {
        height: 80px;
        opacity: 1;
    }
}

@-webkit-keyframes fadeIn {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}

@keyframes fadeIn {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}

@media screen and (max-width: 768px) {
    .menu_inner {
        font-size: 3em;
    }
}
</style>