<template>
    <div class="about_text_wrapper">
        <div class="split_text_container">
            <p v-for="text, i in text_arr" :key="text"
                :class="{'blur_out': active_text == i + 1, 'blur_in': active_text == i}">
                {{text}}</p>
        </div>
    </div>
    <svg id="filters">
        <defs>
            <filter id="c_matrix">
                <feColorMatrix in="SourceGraphic" type="matrix" 
                values="1 0 0 0 0
                        0 1 0 0 0
                        0 0 1 0 0
                        0 0 0 255 -100" />
            </filter>
        </defs>
    </svg>
</template>

<script>
export default {
    name: "AboutText",
    data() {
        return {
            text_arr: ["Hello, this is a test", "How does it look", "I hope it looks good", "I would like to see it", "I hope it works"],
            active_text: 0,
        }
    },
    mounted() {
        setTimeout(() => {
            this.active_text = 1;
            setTimeout(() => {
                this.active_text = 2;
                setTimeout(() => {
                    this.active_text = 3;
                    setTimeout(() => {
                        this.active_text = 4;
                        setTimeout(() => {
                            this.active_text = 5;
                        }, 3000);
                    }, 3000);
                }, 3000);
            }, 3000);
        }, 3000);
    }
}
</script>

<style scoped>
.about_text_wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
}

.split_text_container {
    position: absolute;
    top: 20%;
    left: 50%;
    width: 200px;
    transform: translate(-50%, -50%);
    z-index: 1;
    filter: url(#c_matrix) blur(0.5px);
}


.split_text_container p {
    position: absolute;
    font-weight: 1000;
    font-size: 7vw;
    visibility: hidden;
}

.blur_out {
    visibility: visible !important;
    opacity: 1;
    animation: blurOut 1s ease-in forwards;
}

.blur_in {
    visibility: visible !important;
    opacity: 0;
    animation: blurIn 1s ease-out forwards;
}

@keyframes blurOut {
    0% {
        filter: blur(0px);
        opacity: 1;
    }
    70% {
        opacity: 0.5;
    }
    100% {
        filter: blur(8px);
        opacity: 0;
    }
}

@keyframes blurIn {
    0% {
        opacity: 0;
        filter: blur(8px);
    }
    70% {
        opacity: 0.5;
    }
    100% {
        filter: blur(0px);
        opacity: 1;
    }
}
</style>