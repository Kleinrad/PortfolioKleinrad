<template>
    <div class="about_text_wrapper">
        <div class="split_text_container">
            <div v-for="list, i in text_arr" :key="i"
                :class="{'blur_out': i == last_active_text, 'blur_in': active_text == i}">
                <span v-for="textObj in list" :class="{highlight: textObj.highlight}">{{ textObj.text }}</span>    
            </div>
        </div>
    </div>
    <svg id="filters">
        <defs>
            <filter id="c_matrix">
                <feColorMatrix in="SourceGraphic" type="matrix" 
                values="1 0 0 0 0
                        0 1 0 0 0
                        0 0 1 0 0
                        0 0 0 255 -120" />
            </filter>
        </defs>
    </svg>
</template>

<script>
export default {
    name: "AboutText",
    data() {
        return {
            text_arr: [
                [{text: "Always seeking new challenges and", highlight: false},
                {text: "growth", highlight: true}],
                [{text: "Passionate about", highlight: false},
                {text: "innovative", highlight: true},
                {text: ", impactful solutions", highlight: false}],
                [{text: "Committed to", highlight: false},
                {text: "high-quality", highlight: true}],
                [{text: "Avid", highlight: true},
                {text: " learner and problem solver", highlight: false}],
            ],
            active_text: 0,
            last_active_text: -1,
            last_text_change: new Date(),
            textTop: 0,
        }
    },
    props: {
        scrollCount : {
            type: Number,
            default: 0
        }
    },
    mounted() {
    },
    watch: {
        scrollCount: function (newVal, oldVal) {
            let tmp = this.active_text;
            this.active_text = Math.min(Math.floor((newVal+15) / (0.8/this.text_arr.length*100)), this.text_arr.length);
            if ((tmp != this.active_text || Math.abs(this.last_active_text - this.active_text) != 1)
                && (new Date() - this.last_text_change) > 800) {
                this.last_active_text = tmp;
                this.last_text_change = new Date();
            }
            this.textTop = (0.5/this.text_arr.length * this.active_text*10000) + "%";
        }
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
    top: 5%;
    left: 60%;
    width: 50vw;
    height: 1%;
    transform: translate(-50%, v-bind(textTop));
    z-index: 1;
    filter: url(#c_matrix) blur(0.5px);
    transition: transform 0s 0.6s ease-in-out;
}


.split_text_container div {
    position: absolute;
    font-weight: 1000;
    font-size: 5vw;
    visibility: hidden;
}

.split_text_container span {
    margin: .6vw;
}

.highlight {
    color: var(--accent-color);
}

.blur_out {
    visibility: visible !important;
    opacity: 1;
    animation: blurOut 0.7s ease-in forwards;
}

.blur_in {
    visibility: visible !important;
    opacity: 0;
    animation: blurIn 0.7s .6s ease-out forwards;
}

@keyframes blurOut {
    0% {
        filter: blur(0px);
        opacity: 1;
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
    100% {
        filter: blur(0px);
        opacity: 1;
    }
}

@media screen and (max-width: 768px) {
    .split_text_container div {
        font-size: 9.5vw;
    }
    .split_text_container {
        left: 50%;
        width: 80vw;
    }
}
</style>