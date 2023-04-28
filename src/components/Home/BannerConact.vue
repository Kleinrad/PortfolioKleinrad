<template>
    <a href="mailto:fabiankleinrad.fk@gmail.com">
        <div class="banner_contact" @mouseover="onHover" @mouseleave="onLeave">
                <span :class="{
                    'hover_circle': hover,
                    'circle': true,
                }"></span>
                <span :class="{
                    'hover_text': hover
                }">Reach out to me</span>
            <ArrowButton :hover="hover" :screenType="screenType"></ArrowButton>
            
        </div>
    </a>
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
    props: {
        'screenType': Number,
    },
    mounted() {
    },
    methods: {
        onHover() {
            this.hover = true;
        },
        onLeave(){
            this.hover = false;
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

    display: -webkit-box;

    display: -ms-flexbox;

    display: flex;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    color: var(--font-color) !important;

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
    -webkit-transform: translate(-30%, 10%);
        -ms-transform: translate(-30%, 10%);
            transform: translate(-30%, 10%);
    -webkit-transition: 1s ease;
    -o-transition: 1s ease;
    transition: 1s ease;
}


.hover_text {
    position: relative;
    color: var(--font-color) !important;
    -webkit-transition: 0.2s ease;
    -o-transition: 0.2s ease;
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
    -webkit-animation: underline 0.5s ease forwards;
            animation: underline 0.5s ease forwards;
}

.hover_circle {
    -webkit-transform: scale(.8) translate(-40%, 10%);
        -ms-transform: scale(.8) translate(-40%, 10%);
            transform: scale(.8) translate(-40%, 10%);; 
}



@-webkit-keyframes underline {
    0% {
        width: 0%;
    }
    100% {
        width: 100%;
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

@media screen and (max-width: 768px) {
    .banner_contact {
        bottom: 15%;
        left: 5%;
        font-size: 3.5vw;
    }
    .circle{
        width: 20vw;
        height: 20vw;
        -webkit-transform: translate(-30%, 10%);
            -ms-transform: translate(-30%, 10%);
                transform: translate(-30%, 10%);
    }
    .hover_circle {
        -webkit-transform: scale(.8) translate(-40%, 10%);
            -ms-transform: scale(.8) translate(-40%, 10%);
                transform: scale(.8) translate(-40%, 10%);; 
    }
}
</style>