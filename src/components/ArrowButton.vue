<template>
    <div class="arrow_turn">
        <span :class="{
            'hover_arrow': hover,
            'arrow': true,
        }">&#129130;</span>
        <span :class="{
            'hover_arrow': hover,
            'arrow': true,
        }">&#129130;</span>
    </div>
</template>

<script>
export default {
    name: "ArrowButton",
    data() {
        return {
        };
    },
    props: {
        'hover': Boolean,
    },
};
</script>

<style scoped>
.arrow_turn {
    display: flex;
    margin-left: 1vw;
}

.arrow {
    font-size: 1.5vw;
    transition: 1s;
}

.arrow:first-child {
    transform: rotate3d(0, 1, 0, -90deg);
    opacity: 0;
    color: var(--primary-color);
}

.arrow:last-child {
    transform: rotate3d(0, 1, 0, 0deg) translate(-50%, 0);
    opacity: 1;
}

.hover_arrow:first-child {
    transform: rotate3d(0, 1, 0, 0deg);
    opacity: 1;
}

.hover_arrow:last-child {
    transform: rotate3d(0, 1, 0, 90deg);
    opacity: 0;
}
</style>