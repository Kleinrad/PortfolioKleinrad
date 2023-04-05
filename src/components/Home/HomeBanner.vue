<template>
    <div class="banner_wrapper">
        <HomeTyping/>
        <HomeCarousel/>
    </div>
</template>

<script>
import HomeTyping from "@/components/Home/HomeTyping.vue";
import HomeCarousel from "@/components/Home/HomeCarousel.vue";

export default {
    name: "HomeBanner",
    data() {
        return {
        };
    },
    components: {
        HomeTyping,
        HomeCarousel,
    },
};
</script>

<style scoped>
.banner_wrapper {
    position: absolute;
    left: 0;
    width: 100vw;
    z-index: 0;
    overflow: hidden;
    background-color: #fff;
}

</style>