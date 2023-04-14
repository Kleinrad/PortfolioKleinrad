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
        animation-delay: 0.03s * $i !important;
    }
}

.menu_point {
    position: absolute;
    top: 48%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    justify-content: center;
    align-items: center;
}

.side_menu_text {
    transform-origin: center center;
    padding: 20px;
    transition: 0.2s ease-in-out;
    opacity: 0;
    transform: rotate(90deg);
    animation: rotateBack 0.1s forwards, fade-up 0.5s forwards;

    &.hover {
        transform: rotate(0deg);
        opacity: 1;
        animation: rotate 0.1s forwards;
    }
}

@keyframes rotate {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(90deg);
    }
}

@keyframes rotateBack {
    0% {
        transform: rotate(90deg);
    }
    100% {
        transform: rotate(0deg);
    }
}

.fade_down {
    visibility: hidden;
    animation: fade-down 0.5s forwards;
}

@keyframes fade-down {
    0% {
        opacity: 1;
        transform: translate(0, 0);
    }
    100% {
        opacity: 0;
        transform: translate(0, 20px);
    }
}

@keyframes fade-up {
    0% {
        opacity: 0;
        transform: translate(0, 20px);
    }
    100% {
        opacity: 1;
        transform: translate(0, 0);
    }    
}
</style>