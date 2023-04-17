<template>
    <div class="tech_item_wrapper">
        <div class="tech_logo">
        </div>
    </div>
</template>

<script>
export default {
    name: "TechnologyItem",
    data() {
        return {
            color: "#A1CEB3",
            cssUrl: "",
            letter_count: 0,
        };
    },
    props: {
        name: String,
        logo: String,
    },
    mounted() {
        this.letter_count = this.name.length;
        this.cssUrl = "url(" + this.logo + ")";
    },
};

</script>

<style>
.tech_item_wrapper {
    display: inline-block;
    width: 5vw;
    height: 3.5vw;
    line-height: 3.5vw;
}


.tech_logo {
    position: relative;
    width: 100%;
    height: 100%;
    background-image: v-bind(cssUrl);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
}

.tech_item_wrapper:hover .tech_logo::before {
    animation: fadeOut 0.5s forwards;
}

/* .tech_logo::before {
    content: "";
    display: block;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    opacity: 0;
    background-color: #5affcb;
    mix-blend-mode: hue;
    animation: fadeIn 0.5s forwards;
} */

@keyframes fadeOut {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0;
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
</style>