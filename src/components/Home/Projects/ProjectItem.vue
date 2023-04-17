<template>
    <div id="pItem" :class="{'project_item':true, 'project_item_active': fullSize}" @click="fullSize = !fullSize">
        <span id="cover"></span>
        <img id="cover" :class="{'image_background' : project.media.includes('webp') && fullSize}" v-if="project.media.includes('webp') && fullSize" :src="project.media" alt="project image">
        <iframe
            v-if="fullSize && project.media.includes('youtube')"
            :class="{'project_video': fullSize}"
            :src="project.media"
            frameborder="0"
            allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen></iframe>
        <span :class="{'project_name':true, 'project_name_active':fullSize}">{{ project.name }}</span>
        <div class="project_content" v-if="fullSize">
            <div class="project_technologies">
                <TechnologyItem v-for="technology in project.technologies" :key="technology"
                :name="technology" :logo="logoUrl[technology]"></TechnologyItem>
            </div>
            <span class="project_text">{{ project.text }}</span>
            <div class="project_links">
                <a v-for="link, name in project.links" :key="name" :href="link" target="_blank">{{ name }}</a>
            </div>
        </div>
    </div>
</template>

<script>
import TechnologyItem from "@/components/TechnologyItem.vue";
import logoUrl from "@/assets/logos.json";
 
export default {
    name: "ProjectItem",
    data() {
        return {
            logoUrl: logoUrl
        };
    },
    props: {
        "project": Object,
        "fullSize": {
            type: Boolean,
            default: false,
        },
        "maxWidth": String,
        "maxHeight": String,
    },
    components: {
        TechnologyItem,
    },
};
</script>

<style scoped>

.image_background {
    background-size: cover;
    background-position: center;
    opacity: 0;
    animation: fadeIn 0.5s 1.8s forwards;
    filter: brightness(0.3) saturate(0.85);
}
.project_item {
    position: relative;
    width: 40vw;
    height: 20vw;
    overflow: hidden;
    cursor: pointer;
    background-color: var(--secondary-color);
    transition: 0.5s;
    display: flex;
    justify-content: center;
    align-items: center;
    animation: morph-size-reverse 2s forwards;
}

.project_content {
    position: relative;
    box-sizing: border-box;
    padding: 2vw;
    width: 40vw;
    height: 20vw;
    margin-top: 3vw;
    border-radius: 10%;
    background-color: rgba(16, 16, 16, 0.531);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    mix-blend-mode: normal;
    cursor: default;
    opacity: 0;
    animation: fadeIn 0.5s 3s forwards;
}

.project_item_active {
    width: v-bind(maxWidth);
    height: v-bind(maxHeight);
    box-shadow: 0px 4px 100px #5affcb1f;
    transition: width 0.5s ease-in-out, height 0.5s ease-in-out, box-shadow 1s 0.5s ease-in-out;
    animation: morph-size 2s forwards, 20s morph 2s alternate-reverse infinite !important;
}

.project_name{
    position: absolute;
    z-index: 2;
    font-size: 2.5vw;
    font-weight: 600;
    color: var(--font-color);
}

.project_name_active{
    font-size: 3.5vw !important;
    animation: slide-up 0.8s 2s forwards cubic-bezier( 0.17, 0, 0.38, 1.01 );
}

.project_name_active::before{
    content: "";
    position: absolute;
    bottom: 0;
    left: 50%;
    width: 0%;
    height: 2px;
    background-color: var(--accent-color);
    animation: center-expand 0.5s 2.6s forwards ease-out;
}

.project_technologies {
    display: flex;
    flex-wrap: wrap;
    background-color: rgba(105, 105, 105, 0.582);
    box-sizing: border-box;
    padding: 0.5vw;
    border-radius: 5%;
    justify-content: space-evenly;
}

.project_technologies a{
    margin: 0.5vw;
}

.project_links {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
}

.project_text {
    box-sizing: border-box;
    padding: 0 2vw;
    font-size: 1.1vw;
    font-weight:  600;
    line-height: 1.6vw;
    opacity: 0.9;
}

#cover {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    z-index: 0;
    background-color: transparent;
    cursor: default;
}

.project_video {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 135%;
    z-index: -1;
    opacity: 0;
    animation: fadeIn 0.5s 1.8s forwards;
    filter: brightness(0.3) saturate(0.85);
}

@keyframes morph {
    0%, 100%{
        border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%;
    }
    10%{
        border-radius: 30% 40% 10% 5% / 5% 10% 40% 30%;
    }
    20%{
        border-radius: 40% 50% 5% 10% / 10% 5% 50% 40%;
    }
    30%{
        border-radius: 50% 40% 10% 15% / 15% 10% 60% 50%;
    }
    40%{
        border-radius: 30% 30% 15% 20% / 50% 20% 10% 15%;
    }
    50%{
        border-radius: 40% 50% 20% 25% / 25% 20% 50% 40%;
    }
    60%{
        border-radius: 50% 20% 25% 30% / 30% 25% 20% 50%;
    }
    70%{
        border-radius: 30% 40% 30% 35% / 35% 30% 40% 30%;
    }
    80%{
        border-radius: 40% 50% 35% 40% / 40% 35% 50% 40%;
    }
    90%{
        border-radius: 50% 20% 40% 45% / 45% 40% 20% 50%;
    }
}

@keyframes morph-size{
    0%{
        border-radius: 0%;
        transform: scale(1);
    }
    20%{
        border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
        transform: scale(0.5);
    }
    35%{
        border-radius: 40% 36% 43% 34% / 40% 36% 43% 34%;
    }
    60%{
        border-radius: 35% 28% 16% 7% / 35% 28% 16% 7%;
    }
    100%{
        border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%;
        transform: scale(1);
    }

}

@keyframes morph-size-reverse{
    0%{
        border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%;
        transform: scale(1);
    }
    20%{
        border-radius: 35% 28% 16% 7% / 35% 28% 16% 7%;
    }
    35%{
        border-radius: 40% 36% 43% 34% / 40% 36% 43% 34%;
    }
    60%{
        border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
        transform: scale(0.8);
    }
    100%{
        border-radius: 0%;
        transform: scale(1);
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

@keyframes slide-up {
    0% {
        transform: translateY(0%);
    }
    100% {
        transform: translateY(-300%);
    }
}

@keyframes center-expand {
    0% {
        width: 0%;
        transform: translateX(-50%)
    }
    100% {
        width: 130%;
        transform: translateX(-50%)
    }
}
</style>