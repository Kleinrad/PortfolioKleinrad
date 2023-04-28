<template>
    <div class="about_wrapper">
        <TimeLine v-if="visible" :screenType="screenType"></TimeLine>
        <AboutText v-if="visible" :scrollCount="scrollCount" :screenType="screenType" ></AboutText>
        <div class="about_more">
            <p>The hero section has been replicated from <a href="https://sharobella.at">#sharobella</a> due to an application. This will be replaced in the near future</p>
        </div>
    </div>
</template>

<script>
import TimeLine from "@/components/TimeLine.vue";
import AboutText from "@/components/AboutText.vue";

export default {
    name: "AboutMe",
    components: {
        TimeLine,
        AboutText
    },
    data () {
        return {
        }
    },
    props: {
        visible: {
            type: Boolean,
            default: false
        },
        screenType: {
            type: Number,
            default: 0
        },
        scrollCount: {
            type: Number,
            default: 0
        }
    },
    mounted () {
    },
}

</script>

<style scoped>
.about_wrapper {
    position: relative;
    width: 100%;
    height: 200svh;
    z-index: 1;
}

.about_more {
    position: absolute;
    bottom: 0%;
    left: 50%;
    -webkit-transform: translate(-50%, 0%);
        -ms-transform: translate(-50%, 0%);
            transform: translate(-50%, 0%);
    width: 100vw;
    height: 10vw;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    padding-top: 20vw;
    background: -webkit-gradient(linear, left bottom, left top, color-stop(50%, #080E0E), to(#080E0E00));
    background: -o-linear-gradient(bottom, #080E0E 50%, #080E0E00 100%);
    background: linear-gradient(0deg, #080E0E 50%, #080E0E00 100%);
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
}

.about_more p {
    font-size: 1vw;
    opacity: 0.7;
    width: 50vw;
    margin-bottom: 2vw;
}

.more_links {
    margin-bottom: 1.5vw;
}

@media screen and (max-width: 1050px) {
    .about_wrapper {
        height: 250svh;
    }
    .about_more {
        padding-top: 17vw;
    }
    .about_more p {
        font-size: 2vw;
    }
}

@media screen and (max-width: 768px) {
    .about_more {
        padding-top: 35vw;
        height: 60vw;
    }

    .about_more p {
        font-size: 3.5vw;
        width: 80vw;
    }
}
</style>