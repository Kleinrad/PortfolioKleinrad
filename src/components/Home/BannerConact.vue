<template>
    <div class="banner_contact" @mouseover="onHover" @mouseleave="onLeave">
        <span :class="{
            'hover_circle': hover,
            'circle': true,
        }"></span>
        <span :class="{
            'hover_text': hover
        }">Reach out to me</span>
        <ArrowButton :hover="hover"></ArrowButton>
    </div>

</template>

<script>
import ArrowButton from "@/components/ArrowButton.vue"

export default {
    name: "BannerContact",
    data() {
        return {
            hover: false,
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
    components: {
        ArrowButton,
    },
};
</script>

<style scoped>
.banner_contact {
    position: absolute;
    margin-top: 2.5vw;
    margin-left: 7vw;

    display: flex;
    align-items: center;

    font-size: 1.1vw;
    font-weight: 700;
    cursor: pointer;
}

.circle{
    content: "";
    position: absolute;
    z-index: -1;
    width: 6vw;
    height: 6vw;
    border-radius: 50%;
    opacity: 0.7;
    background-color: var(--accent-color);
    transform: translate(-30%, 10%);
    transition: 1s ease;
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



@keyframes underline {
    0% {
        width: 0%;
    }
    100% {
        width: 100%;
    }
}
</style>