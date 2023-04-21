<template>
    <div class="about_wrapper">
        <TimeLine v-if="visible"></TimeLine>
        <AboutText v-if="visible"></AboutText>
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
</style>