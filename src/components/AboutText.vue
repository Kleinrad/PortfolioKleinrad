<template>
    <div class="about_text_wrapper">
        <div :class="{'split_text_container':true, 'split_text_container_notMobile':screenType!=0}">
            <div v-for="list, i in text_arr" :key="i"
                :class="{'blur_out': i == last_active_text, 'blur_in': active_text == i}">
                <span v-for="textObj in list" :class="{highlight: textObj.highlight}">{{ textObj.text }}</span>    
            </div>
        </div>
        <Transition name="fade">
            <div class="contact_wrapper" v-if="active_text == text_count-1">
                <a href="mailto:fabiankleinrad.fk@gmail.com" >
                    <div class="contact_me">
                        get in touch
                    </div>
                </a>
            </div>
        </Transition>
        <!-- <div class="cite">
            <span>The only thing that is constant is change.</span>
            <span> - Heraclitus</span>
        </div> -->
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
                [{text: "I'm a developer with a", highlight: false},
                {text: " passion", highlight: true},
                {text: " of conquering new", highlight: false},
                {text: " challenges", highlight: true}],
                [{text: "I ", highlight: false},
                {text: "love", highlight: true},
                {text: " to surprise people with what's", highlight: false},
                {text: " possible", highlight: true}],
                [{text: "And constantly strive to improve my ", highlight: false},
                {text: " knowledge ", highlight: true},
                {text: "and", highlight: false},
                {text: " skills", highlight: true}],
                [{text: "Let's see what we can", highlight: false},
                {text: " create ", highlight: true},
                {text: "together", highlight: false}],
            ],
            active_text: 0,
            last_active_text: -1,
            last_text_change: new Date(),
            textTop: 0,
            textTopMax: 0,
            text_count: 0
        }
    },
    props: {
        scrollCount : {
            type: Number,
            default: 0
        },
        screenType: {
            type: Number,
            default: 0
        }
    },
    mounted() {
        this.text_count = this.text_arr.length;
        setTimeout(() => {
            this.textTopMax = ((this.screenType==0?0.7:0.5)/this.text_arr.length * (this.text_arr.length-1)*10000) + "%";
        }, 100);
    },
    watch: {
        scrollCount: function (newVal, oldVal) {
            let tmp = this.active_text;
            this.active_text = Math.min(Math.floor((newVal+15) / (0.9/this.text_arr.length*100)), this.text_arr.length-1);
            if ((tmp != this.active_text || Math.abs(this.last_active_text - this.active_text) != 1)
                && (new Date() - this.last_text_change) > 800) {
                this.last_active_text = tmp;
                this.last_text_change = new Date();
            }
            this.textTop = ((this.screenType==0?0.7:0.5)/this.text_arr.length * this.active_text*10000) + "%";
        }
    }
}
</script>

<style scoped>

.fade-enter-active {
  -webkit-transition: opacity 0.2s .7s ease !important;
  -o-transition: opacity 0.2s .7s ease !important;
  transition: opacity 0.2s .7s ease !important;
}

.fade-leave-active {
  -webkit-transition: opacity 0.2s .2s ease !important;
  -o-transition: opacity 0.2s .2s ease !important;
  transition: opacity 0.2s .2s ease !important;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0 !important;
}

.about_text_wrapper {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
}

.split_text_container_notMobile{
    -webkit-filter: url(#c_matrix) blur(0.5px) !important;
            filter: url(#c_matrix) blur(0.5px) !important;
}

.split_text_container {
    position: absolute;
    top: 5%;
    left: 60%;
    width: 50vw;
    height: 1%;
    -webkit-transform: translate(-50%, v-bind(textTop));
        -ms-transform: translate(-50%, v-bind(textTop));
            transform: translate(-50%, v-bind(textTop));
    z-index: 1;
    -webkit-filter: url(#c_matrix) blur(0.1px);
            filter: url(#c_matrix) blur(0.1px);
    -webkit-transition: -webkit-transform 0s 0.5s ease-in-out;
    transition: -webkit-transform 0s 0.5s ease-in-out;
    -o-transition: transform 0s 0.5s ease-in-out;
    transition: transform 0s 0.5s ease-in-out;
    transition: transform 0s 0.5s ease-in-out, -webkit-transform 0s 0.5s ease-in-out;
}

.cite {
    position: absolute;
    left: 32%;
    -webkit-transform: translate(-50%, 0%);
        -ms-transform: translate(-50%, 0%);
            transform: translate(-50%, 0%);
    bottom: 0;
    margin-bottom: 25vw;
    font-size: 3vw;
    font-weight: 400;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    color: var(--font-color);
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    mix-blend-mode: difference;
    opacity: 0.8;
}

.cite span:last-child {
    font-size: 2vw;
    margin-top: 1vw;
    opacity: 0.5;
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
    -webkit-animation: blurOut 0.6s ease-in forwards;
            animation: blurOut 0.6s ease-in forwards;
}

.blur_in {
    visibility: visible !important;
    opacity: 0;
    -webkit-animation: blurIn 0.6s .5s ease-out forwards;
            animation: blurIn 0.6s .5s ease-out forwards;
}

.contact_wrapper {
    position: absolute;
    top: 30vw;
    z-index: 0;
    right: 18%;
    height: 1%;
    -webkit-transform: translate(-50%, v-bind(textTopMax));
        -ms-transform: translate(-50%, v-bind(textTopMax));
            transform: translate(-50%, v-bind(textTopMax));
    -webkit-transition: -webkit-transform 0s 0.5s ease-in-out;
}

.contact_me {
    position: relative;
    font-size: 1.5vw;
    color: var(--accent-color);
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    padding: 13% 13%;
    width: 12vw;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    pointer-events: all;
    border-radius: 5vw;
    background-color: var(--secondary-color);
    opacity: 0.9;
    -webkit-transition: 0.3s ease-in-out;
    -o-transition: 0.3s ease-in-out;
    transition: 0.3s ease-in-out;
}


.contact_me::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 5vw;
    background: -webkit-gradient(linear, left bottom, left top, from(#080E0Ef0),  to(#080E0Eb0));
    background: -o-linear-gradient(bottom, #080E0Ef0 0%,  #080E0Eb0 100%);
    background: linear-gradient(0deg, #080E0Ef0 0%,  #080E0Eb0 100%);
    -webkit-transform: translate(-2%, -2%);
        -ms-transform: translate(-2%, -2%);
            transform: translate(-2%, -2%);
    opacity: 1;
    z-index: 1;
    -webkit-animation: reveal 1.58s 0.8s ease-in-out forwards;
            animation: reveal 1.58s 0.8s ease-in-out forwards;
}

.contact_me::after {
    position: absolute;
    content: "";
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    background-color: transparent;
    border-radius: 5vw;
    opacity: 0;
    -webkit-transition: 1s;
    -o-transition: 1s;
    transition: 1s;
    -webkit-animation: border-rotate 1s 0.8s;
            animation: border-rotate 1s 0.8s;
}

.contact_me:hover::after {
    opacity: 1 !important;
    -webkit-box-shadow: 4px 0px 2px 0px var(--accent-color);
            box-shadow: 4px 0px 2px 0px var(--accent-color);
    -webkit-animation: border-rotate 1s infinite linear;
            animation: border-rotate 1s infinite linear;
}

.contact_me:hover {
    border: none;
    -webkit-transform: translate(-50%, -50%) scale(1.03);
        -ms-transform: translate(-50%, -50%) scale(1.03);
            transform: translate(-50%, -50%) scale(1.03);
}

@-webkit-keyframes reveal {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0.6;
    }
}

@keyframes reveal {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0.6;
    }
}

@-webkit-keyframes border-rotate {
    0% {
        opacity: 0.1;
        -webkit-box-shadow: 4px 0px 2px 0px var(--accent-color);
                box-shadow: 4px 0px 2px 0px var(--accent-color);
    }
    10% {
        opacity: 0.2;
        -webkit-box-shadow: 4px 2px 4px 0px var(--accent-color);
                box-shadow: 4px 2px 4px 0px var(--accent-color);
    }
    25% {
        opacity: 0.4;
        -webkit-box-shadow: 0px 4px 2px 0px var(--accent-color);
                box-shadow: 0px 4px 2px 0px var(--accent-color);
    }
    40% {
        opacity: 0.6;
        -webkit-box-shadow: -2px 3px 4px 0px var(--accent-color);
                box-shadow: -2px 3px 4px 0px var(--accent-color);
    }
    50% {
        opacity: 0.8;
        -webkit-box-shadow: -4px 0px 2px 0px var(--accent-color);
                box-shadow: -4px 0px 2px 0px var(--accent-color);
    }
    65% {
        opacity: 1;
        -webkit-box-shadow: -3px -2px 4px 0px var(--accent-color);
                box-shadow: -3px -2px 4px 0px var(--accent-color);
    }
    80% {
        opacity: 0.5;
        -webkit-box-shadow: 0px -4px 2px 0px var(--accent-color);
                box-shadow: 0px -4px 2px 0px var(--accent-color);
    }
    100% {
        opacity: 0.1;
        -webkit-box-shadow: 4px 0px 2px 0px var(--accent-color);
                box-shadow: 4px 0px 2px 0px var(--accent-color);
    }
}

@keyframes border-rotate {
    0% {
        opacity: 0.1;
        -webkit-box-shadow: 4px 0px 2px 0px var(--accent-color);
                box-shadow: 4px 0px 2px 0px var(--accent-color);
    }
    10% {
        opacity: 0.2;
        -webkit-box-shadow: 4px 2px 4px 0px var(--accent-color);
                box-shadow: 4px 2px 4px 0px var(--accent-color);
    }
    25% {
        opacity: 0.4;
        -webkit-box-shadow: 0px 4px 2px 0px var(--accent-color);
                box-shadow: 0px 4px 2px 0px var(--accent-color);
    }
    40% {
        opacity: 0.6;
        -webkit-box-shadow: -2px 3px 4px 0px var(--accent-color);
                box-shadow: -2px 3px 4px 0px var(--accent-color);
    }
    50% {
        opacity: 0.8;
        -webkit-box-shadow: -4px 0px 2px 0px var(--accent-color);
                box-shadow: -4px 0px 2px 0px var(--accent-color);
    }
    65% {
        opacity: 1;
        -webkit-box-shadow: -3px -2px 4px 0px var(--accent-color);
                box-shadow: -3px -2px 4px 0px var(--accent-color);
    }
    80% {
        opacity: 0.5;
        -webkit-box-shadow: 0px -4px 2px 0px var(--accent-color);
                box-shadow: 0px -4px 2px 0px var(--accent-color);
    }
    100% {
        opacity: 0.1;
        -webkit-box-shadow: 4px 0px 2px 0px var(--accent-color);
                box-shadow: 4px 0px 2px 0px var(--accent-color);
    }
}

@-webkit-keyframes blurOut {
    0% {
        -webkit-filter: blur(0px);
                filter: blur(0px);
        opacity: 1;
    }
    100% {
        -webkit-filter: blur(8px);
                filter: blur(8px);
        opacity: 0;
    }
}

@keyframes blurOut {
    0% {
        -webkit-filter: blur(0px);
                filter: blur(0px);
        opacity: 1;
    }
    100% {
        -webkit-filter: blur(8px);
                filter: blur(8px);
        opacity: 0;
    }
}

@-webkit-keyframes blurIn {
    0% {
        opacity: 0;
        -webkit-filter: blur(8px);
                filter: blur(8px);
    }
    100% {
        -webkit-filter: blur(0px);
                filter: blur(0px);
        opacity: 1;
    }
}

@keyframes blurIn {
    0% {
        opacity: 0;
        -webkit-filter: blur(8px);
                filter: blur(8px);
    }
    100% {
        -webkit-filter: blur(0px);
                filter: blur(0px);
        opacity: 1;
    }
}

@media screen and (max-width: 1050px) {
    .contact_me {
        -webkit-transform: translate(-50%, -50%) scale(1.5);
            -ms-transform: translate(-50%, -50%) scale(1.5);
                transform: translate(-50%, -50%) scale(1.5);
        font-size: 2vw;
        opacity: .98;
        padding: 10% 10%;
        width: 15vw;
    }

    .contact_wrapper {
        top: 38vw;
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

    .cite {
        left: 50%;
        font-size: 10vw;
        margin-bottom: 50vw;
    }

    .cite span:last-child{
        font-size: 6vw;
    }

    .contact_me {
        -webkit-transform: translate(-50%, -50%) scale(2);
            -ms-transform: translate(-50%, -50%) scale(2);
                transform: translate(-50%, -50%) scale(2);
        font-size: 3vw;
        left: 50%;
        opacity: .98;
        width: 30vw;
    }
    .contact_wrapper {
        top: 75vw;
    }
    .contact_me:hover {
        -webkit-transform: translate(-50%, -50%) scale(2.03);
            -ms-transform: translate(-50%, -50%) scale(2.03);
                transform: translate(-50%, -50%) scale(2.03);
    }
}

</style>