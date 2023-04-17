<template>
    <div class="tech_item_wrapper">
        <div class="tech_logo">
            <div class="tech_color"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TechnologyItem",
    data() {
        return {
            name: "C++",
            logo: "https://upload.wikimedia.org/wikipedia/commons/1/18/ISO_C%2B%2B_Logo.svg",
            color: "#A1CEB3",
            letter_count: 0,
        };
    },
    mounted() {
        this.letter_count = this.name.length;
        this.logo = "url(" + this.logo + ")";
    },
};

</script>

<style>
.tech_item_wrapper {
    position: relative;
    display: inline-block;
    width: 4vw;
    height: 4vw;
    line-height: 4vw;
    overflow: hidden;
}


.tech_logo {
    width: 4vw;
    height: 4vw;
    background-image: v-bind(logo);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
}

.tech_item_wrapper:hover .tech_color {
    animation: shrink 0.5s forwards;
}

.tech_color {
    width: 100%;
    height: 100%;
    background-color: #A1CEB3;
    mix-blend-mode: color;
}

@keyframes shrink {
    0% {
        height: 4vw;
    }
    100% {
        height: 0vw;
    }
}
</style>