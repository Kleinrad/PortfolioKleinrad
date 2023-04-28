<template>
    <div class="menu_point">
        <span v-for="letter in point" :key="letter" 
            :class="{'side_menu_text': true, 'hover': isHovered}">{{letter}}</span>
    </div>
</template>

<script>
export default {
    data() {
        return {
        }
    },
    props: {
        point: String,
        isHovered: Boolean,
    },
}
</script>

<style scoped lang="scss">

@for $i from 1 through 20 {
    .side_menu_text:nth-child(#{$i}) {
        -webkit-animation-delay: 0.03s * $i !important;
                animation-delay: 0.03s * $i !important;
    }
}

.menu_point {
    position: absolute;
    top: 48%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
}

.side_menu_text {
    -webkit-transform-origin: center center;
        -ms-transform-origin: center center;
            transform-origin: center center;
    padding: 20px;
    -webkit-transition: 0.2s ease-in-out;
    -o-transition: 0.2s ease-in-out;
    transition: 0.2s ease-in-out;
    opacity: 0;
    -webkit-transform: rotate(90deg);
        -ms-transform: rotate(90deg);
            transform: rotate(90deg);
    -webkit-animation: rotateBack 0.1s forwards, fade-up 0.5s forwards;
            animation: rotateBack 0.1s forwards, fade-up 0.5s forwards;

    &.hover {
        -webkit-transform: rotate(0deg);
            -ms-transform: rotate(0deg);
                transform: rotate(0deg);
        opacity: 1;
        -webkit-animation: rotate 0.1s forwards;
                animation: rotate 0.1s forwards;
    }
}

@-webkit-keyframes rotate {
    0% {
        -webkit-transform: rotate(0deg);
                transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(90deg);
                transform: rotate(90deg);
    }
}

@keyframes rotate {
    0% {
        -webkit-transform: rotate(0deg);
                transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(90deg);
                transform: rotate(90deg);
    }
}

@-webkit-keyframes rotateBack {
    0% {
        -webkit-transform: rotate(90deg);
                transform: rotate(90deg);
    }
    100% {
        -webkit-transform: rotate(0deg);
                transform: rotate(0deg);
    }
}

@keyframes rotateBack {
    0% {
        -webkit-transform: rotate(90deg);
                transform: rotate(90deg);
    }
    100% {
        -webkit-transform: rotate(0deg);
                transform: rotate(0deg);
    }
}

.fade_down {
    visibility: hidden;
    -webkit-animation: fade-down 0.5s forwards;
            animation: fade-down 0.5s forwards;
}

@-webkit-keyframes fade-down {
    0% {
        opacity: 1;
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }
    100% {
        opacity: 0;
        -webkit-transform: translate(0, 20px);
                transform: translate(0, 20px);
    }
}

@keyframes fade-down {
    0% {
        opacity: 1;
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }
    100% {
        opacity: 0;
        -webkit-transform: translate(0, 20px);
                transform: translate(0, 20px);
    }
}

@-webkit-keyframes fade-up {
    0% {
        opacity: 0;
        -webkit-transform: translate(0, 20px);
                transform: translate(0, 20px);
    }
    100% {
        opacity: 1;
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }    
}

@keyframes fade-up {
    0% {
        opacity: 0;
        -webkit-transform: translate(0, 20px);
                transform: translate(0, 20px);
    }
    100% {
        opacity: 1;
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }    
}

@media screen and (max-width: 768px) {
    .menu_point {
        font-size: 5vw;
        left: 5%;
        -webkit-transform: translate(0%, -50%);
            -ms-transform: translate(0%, -50%);
                transform: translate(0%, -50%);
    }

    .side_menu_text{
        padding: 10px;
    }
}
</style>