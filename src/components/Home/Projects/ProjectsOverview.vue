<template>
    <div class="projects_wrapper">
        <div class="item_wrapper" v-for="project in projects">
            <ProjectItem  :key="project.id" :fullSize="false" :project="project"></ProjectItem>
        </div>
    </div>
</template>

<script>
import ProjectItem from "@/components/Home/Projects/ProjectItem.vue";
import MapReduceImg from "@/assets/images/MapReduce.webp";

export default {
    name: "ProjectsOverview",
    data() {
        return {
            projects: [
                {
                    id: 1,
                    name: "Autumn",
                    media: "https://www.youtube.com/embed/OxoY1ocx72U?controls=1&mute=1&rel=0&autoplay=1&loop=1&playlist=OxoY1ocx72U",
                }, 
                {
                    id: 2,
                    name: "MapReduce",
                    media: MapReduceImg,
                }, 
                {
                    id: 3,
                    name: "AmongHTL",
                    media: "",
                }, 
            ]
        };
    },
    components: {
        ProjectItem,
    },
};
</script>

<style scoped>
.projects_wrapper {
    position: relative;
    width: 100%;
    height: calc(45vw*3);
    background-color:  var(--background-color);
}

.item_wrapper {
    position: relative;
    width: 100%;
    height: 35vw;
    padding: 5vw  0;
    display: flex;
    justify-content: center;
    align-items: center;
}

/* shift all even 20% right and all odd 20% left */
.item_wrapper:nth-child(odd) {
    transform: translateX(12%);
}

.item_wrapper:nth-child(even) {
    transform: translateX(-12%);
}


</style>