<template>
    <div class="banner_typing">
        <span>I #</span>
        <div id="typing">{{text}}</div>
        <span v-if="showCursor && cursorBlink" class="typing_cursor">|</span>
    </div>
</template>

<script>
    export default {
        name: "HomeBanner",
        data() {
            return {
                text: "",
                textArray: ["like-design:", "love-coding:", "create-new:"],
                loopNum: 0,
                typingSpeed: 100,
                isDeleting: false,
                showCursor: true,
                cursorBlink: true,
            };
        },
        mounted() {
            setTimeout(() => {
                this.typeWriter();
            }, 1000);
        },
        methods: {
            typeWriter() {
                let i = this.loopNum % this.textArray.length;
                let fullText = this.textArray[i];

                if (this.isDeleting) {
                    this.showCursor = false;
                    this.text = fullText.substring(0, this.text.length - 1);
                } else {
                    this.showCursor = true;
                    this.cursorBlink = true;
                    this.text = fullText.substring(0, this.text.length + 1);
                }

                
                let that = this;
                let delta = 200 - Math.random() * 100;

                if (this.isDeleting) {
                    delta /= 1.6;
                }

                if (!this.isDeleting && this.text === fullText) {
                    delta = this.typingSpeed;
                    delta = 3000;
                    this.isDeleting = true;
                } else if (this.isDeleting && this.text === "") {
                    this.isDeleting = false;
                    this.loopNum++;
                    delta = 1500;
                    this.cursorBlink = true;
                    this.showCursor = true;
                } else if(this.isDeleting){
                    setTimeout(function () {
                        that.cursorBlink = false;
                    }, delta-100);
                }

                setTimeout(function () {
                    that.typeWriter();
                }, delta);
            },
        },
    };
</script>

<style scoped>
.banner_typing{
    display: flex;
    align-items: flex-end;
    justify-content: flex-start;
    font-size: 5vw;
    letter-spacing: 0.02em;
    font-weight: bold;
    height: 15vw;
    text-align: left;
    margin-bottom: 2.5vw;
    margin-left: 5vw;
    color: black;
}

.typing_cursor{
    font-weight: 100;
    animation: cursor_blink 1s infinite;
}

@keyframes cursor_blink {
    0% {
        opacity: 0;
    }
    20% {
        opacity: 0;
    }
    25% {
        opacity: 1;
    }
    75% {
        opacity: 1;
    }
    80% {
        opacity: 0;
    }
    100% {
        opacity: 0;
    }
}
</style>