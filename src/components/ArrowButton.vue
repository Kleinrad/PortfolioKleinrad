<template>
    <div class="arrow_turn">
        <span :class="{
            'hover_arrow': hover,
            'arrow': true,
        }" v-html="arrow_char"></span>
        <span :class="{
            'hover_arrow': hover,
            'arrow': true,
        }" v-html="arrow_char"></span>
    </div>
</template>

<script>
export default {
    name: "ArrowButton",
    data() {
        return {
            arrow_char: "&#x1F862;",
        };
    },
    props: {
        'hover': Boolean,
        'screenType': Number,
    },
    mounted() {
        setTimeout(() => {
            this.arrow_char = this.screenType == 0 ? ">" : "&#x1F862;";
        }, 20);
    },
};
</script>

<style scoped>
.arrow_turn {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    margin-left: 1vw;
}

.arrow {
    font-size: 1.5vw;
    -webkit-transition: 0.5s;
    -o-transition: 0.5s;
    transition: 0.5s;
}

.arrow:first-child {
    -webkit-transform: rotate3d(0, 1, 0, -90deg);
            transform: rotate3d(0, 1, 0, -90deg);
    opacity: 0;
    color: var(--accent-color);
}

.arrow:last-child {
    -webkit-transform: rotate3d(0, 1, 0, 0deg) translate(-70%, 0);
            transform: rotate3d(0, 1, 0, 0deg) translate(-70%, 0);
    opacity: 1;
}

.hover_arrow:first-child {
    -webkit-transform: rotate3d(0, 1, 0, 0deg);
            transform: rotate3d(0, 1, 0, 0deg);
    opacity: 1;
}

.hover_arrow:last-child {
    -webkit-transform: rotate3d(0, 1, 0, 90deg);
            transform: rotate3d(0, 1, 0, 90deg);
    opacity: 0;
}

@media screen and (max-width: 768px) {
    .arrow {
        font-size: 5vw;
    }
}
</style>