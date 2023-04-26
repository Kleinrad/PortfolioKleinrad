<template>
    <div class="about_wrapper">
        <TimeLine v-if="visible" :screenType="screenType"></TimeLine>
        <AboutText v-if="visible" :scrollCount="scrollCount"></AboutText>
        <div class="about_more">
            <p>If you liked what you saw reach out to me I would love to see what we can create together</p>
            <div class="more_links">
                <a href="">CV</a>
            </div>
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
    transform: translate(-50%, 0%);
    width: 100vw;
    height: 30vw;
    box-sizing: border-box;
    padding-top: 20vw;
    background: linear-gradient(0deg, #080E0E 50%, #080E0E00 100%);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

.about_more p {
    font-size: 1vw;
    width: 50vw;
}

.more_links {
    margin-bottom: 1.5vw;
}

@media screen and (max-width: 1050px) {
    .about_wrapper {
        height: 250svh;
    }
}
</style>