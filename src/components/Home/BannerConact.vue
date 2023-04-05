<template>
    <div class="banner_contact" @mouseover="onHover" @mouseleave="onLeave">
        <span :class="{
            'hover_circle': hover,
            'circle': true,
        }"></span>
        <span :class="{
            'hover_text': hover
        }">Reach out to me</span>
        <span :class="{
            'hover_arrow': hover,
            'arrow': true,
        }">&#129130;</span>
    </div>

</template>

<script>
export default {
    name: "BannerContact",
    data() {
        return {
            hover: true,
        };
    },
    mounted() {
    },
    methods: {
        onHover() {
            this.hover = true;
            console.log("hover");
        },
        onLeave(){
            this.hover = false;
            console.log("leave");
        },
    },
};
</script>

<style scoped>
.banner_contact {
    position: absolute;
    margin-top: 5vw;
    margin-left: 7vw;

    display: flex;
    align-items: center;

    font-size: 1.1vw;
    font-weight: 700;
}

.circle{
    content: "";
    position: absolute;
    z-index: -1;
    width: 7vw;
    height: 7vw;
    border-radius: 50%;
    opacity: 0.7;
    background-color: var(--primary-color);
    transform: translate(-30%, 10%);
    transition: 1s ease;
}

.arrow{
    font-size: 1.5vw;
    margin-left: 1vw;
    transition: 0.2;
}


.hover_text {
    position: relative;
    transition: 0.2s ease;
}
.hover_text::after {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 1px;
    background-color: black;
    animation: underline 0.5s ease forwards;
}

.hover_circle {
    transform: scale(.8) translate(-40%, 10%);; 
}

.hover_arrow {
    animation: arrow 0.5s ease forwards;
}

@keyframes arrow {
    0% {
        transform: translate3d(0, 0, 0);
        opacity: 1;
    }
    49% {
        transform: translate3d(0.5vw, 0, 0.5vw);
        color: black;
        opacity: 0;
    }
    50% {
        color: var(--primary-color);
        opacity: 0;
        transform: translate3d(-0.5vw, 0, -0.5vw);
    }
    100% {
        transform: translate3d(0, 0, 0);
        color: var(--primary-color);
        opacity: 1;
    }
}

@keyframes underline {
    0% {
        width: 0%;
    }
    100% {
        width: 100%;
    }
}
</style>