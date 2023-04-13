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
                    media: "https://www.youtube.com/embed/OxoY1ocx72U?controls=1&mute=1&rel=0&autoplay=1&loop=1",
                }, 
                {
                    id: 2,
                    name: "MapReduce",
                    media: MapReduceImg,
                }, 
                {
                    id: 3,
                    name: "AmongHTL",
                    media: "https://www.youtube.com/embed/7OwdxQSfSx0?controls=0&autoplay=1&mute=1&loop=1",
                },
            ],
            project_count: 0,
        };
    },
    components: {
        ProjectItem,
    },
    mounted() {
        this.project_count = this.projects.length;
    },
};
</script>

<style scoped>
.projects_wrapper {
    position: relative;
    scroll-snap-align: start;
    width: 100%;
    height: calc(45vw*v-bind(project_count));
    background-color:  var(--background-color);
    transition: var(--bg-transition);
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