<template>
    <div class="banner_wrapper">
        <HomeTyping/>
        <HomeCarousel/>
        <div class="banner_text">
            <span>As a developer, I am eager to showcase my skills and passion for coding. With a focus on web development and a commitment to delivering high-quality solutions.</span>
            <span>Let's create something great together!</span>
        </div>
        <BannerConact :screenType="screenType"/>
    </div>
</template>

<script>
import HomeTyping from "@/components/Home/HomeTyping.vue";
import HomeCarousel from "@/components/Home/HomeCarousel.vue";
import BannerConact from "@/components/Home/BannerConact.vue";

export default {
    name: "HomeBanner",
    data() {
        return {
        };
    },
    props: {
        'screenType': Number,
    },
    components: {
        HomeTyping,
        HomeCarousel,
        BannerConact,
    },
};
</script>

<style scoped>
.banner_wrapper {
    position: relative;
    scroll-snap-align: start;
    height: 100svh;
    width: 100%;
    z-index: 0;
    overflow: hidden;
    background-color: var(--background-color);
    -webkit-transition: var(--bg-transition);
    -o-transition: var(--bg-transition);
    transition: var(--bg-transition);
}

.banner_text{
    width: 45vw;
    height: 10vw;
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
    text-align: left;
    font-weight: 700;
    font-size: 1.6vw;
    margin-left: 5vw;
}

.banner_text span{
    margin: 0.5vw 0;
}

@media screen and (max-width: 768px) {
    .banner_text{
        width: 80vw;
        height: 20vw;
        font-size: 4.5vw;
        font-weight: 600;
    }    
}
</style>