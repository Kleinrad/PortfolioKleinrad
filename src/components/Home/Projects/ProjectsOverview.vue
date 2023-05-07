<template>
    <div class="projects_wrapper" @mouseenter="edgeLine=false" @mouseleave="edgeLine=true">
        <ProjectLine :active="show" :lineLength="lineLength" :projectCount="projects.length" :dotDist="dotDistance" :screenType="screenType"></ProjectLine>
        <div class="item_wrapper" v-for="project, i in projects">
            <ProjectItem :screenType="screenType" :key="project.id" :fullSize="false" :project="project" :full-size="i == activeVideo" maxHeight="30vw" maxWidth="60vw"></ProjectItem>
        </div>
        <div v-if="dark"  class="project_bg"></div>
        <video v-if="dark" id="project_bg_video" autoplay muted loop playbackRate="0.8">
            <source :src="bgVidPath" type="video/mp4"/>
        </video>
    </div>
</template>

<script>
import ProjectItem from "@/components/Home/Projects/ProjectItem.vue";
import ProjectLine from "@/components/Home/Projects/ProjectLine.vue";

export default {
    name: "ProjectsOverview",
    data() {
        return {
            projects: [
                {
                    id: 1,
                    name: "Autumn",
                    media: "https://www.youtube.com/embed/OxoY1ocx72U?controls=1&mute=1&rel=0&autoplay=1&loop=1",
                    text: "Autumn offers an innovative autonomous drone solution aimed at simplifying complex tasks, such as construction and forest monitoring, by generating accurate environment models and enabling real-time progress monitoring.",
                    text_long: "Computers nowadays are integrated into a wide variety of processes. Ranging from computeraided construction to software-based monitoring of forests. Autumn proposes a solution of using an autonomous and universally deployable drone to simplify these tasks. The drone generates a realistic model of the environment, which can be used to extract measurements or capture the momentary progress. Furthermore, Autumn uses technologies that enable the drone to work independently of any external localisation mechanism, which allows for deployment in secluded areas. This characteristic can be realised by using a SLAM algorithm, needed to map the environment in combination with an RRT* algorithm, which handles the path-planning.",
                    technologies: ["C++", "ROS", "GitHub", "LaTeX"],
                    links: {
                        "GitHub": "https://github.com/F-WuTS/Autumn",
                        "Paper": process.env.BASE_URL + "Autumn_Paper.pdf",
                        "Video": "https://www.youtube.com/watch?v=OxoY1ocx72U"
                    }
                }, 
                {
                    id: 2,
                    name: "MapReduce",
                    media: process.env.BASE_URL + "images/MapReduce.webp",
                    text: "The project simulates MapReduce technology using a simple system built with C++17 and meson. It utilizes TCP protocol, asio library, and protocol buffers for efficient data structure serialization.",
                    technologies: ["C++", "Protocol Buffers", "GitHub", "LaTeX"],
                    links: {
                        "GitHub": "https://github.com/Kleinrad/MapReduce_Kleinrad",
                        "Paper": process.env.BASE_URL + "MapReduce_Paper.pdf",
                    }
                }, 
                {
                    id: 3,
                    name: "AmongHTL",
                    media: "https://www.youtube.com/embed/7OwdxQSfSx0?controls=0&autoplay=1&mute=1&loop=1",
                    technologies: ["HTML5", "CSS3", "JavaScript", "Node.js", "Socket.io", "GitHub"],
                    text: "AmongHTL is a web-based multiplayer game, inspired by the popular game Among Us. It is built with HTML5, CSS3, JavaScript, Node.js, and Socket.io.",
                    links: {
                        "GitHub": "https://github.com/AmongHTBLuVA/AmongHTL",
                        //"Game": "localhost:8079"
                    }
                },
                {
                    id: 4,
                    name: "Lines",
                    media: "https://www.youtube.com/embed/7I9TvGfEwtw?controls=0&autoplay=1&mute=1&loop=1",
                    technologies: ["HTML5", "CSS3", "JavaScript"],
                    text: "The purpose of this project is to create visually pleasing and dynamic animations. The inspiration behind this project was laser shows from big festivals.",
                    links: {
                        "Try it out": "https://lines.kleinrad.com"
                    }
                },
                {
                    id: 5,
                    name: "MedQuest",
                    media: process.env.BASE_URL + "images/MedQuest.webp",
                    technologies: ["HTML5", "CSS3", "JavaScript", "GitHub"],
                    text: "MedQuest is a quiz that contains questions from previous MedAT exams. It was an effort to support my girlfriend in her preparation for the MedAT exam.",
                    links: {
                        "GitHub": "https://github.com/Kleinrad/MedQuest",
                        "Try it out": "https://medquest.kleinrad.com"
                    }
                }
            ],
            project_count: 0,
            edgeLine: true,
            activeVideo: -1,
            bgVidPath: process.env.BASE_URL + "ProjectBgVid.mp4",
        };
    },
    props: {
        lineLength: {
            type: Number,
            default: 0,
        },
        dotDistance: {
            type: Number,
            default: 0,
        },
        dark : {
            type: Boolean,
            default: false,
        },
        screenType: {
            type: Number,
            default: 0,
        },
        show: {
            type: Boolean,
            default: true,
        },
    },
    components: {
        ProjectItem,
        ProjectLine,
    },
    mounted() {
        this.project_count = this.projects.length;
    },
    watch: {
        dotDistance: function(newVal, oldVal) {
            let offset = this.screenType == 0 ? -5 : 5;
            this.activeVideo = Math.floor(Math.max(0,newVal-offset)/100 * this.project_count);
            if(newVal < 5) {
                this.activeVideo = -1;
            }
        },
    },
};
</script>

<style scoped>
ProjectLine {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 2;
}

#project_bg_video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    -o-object-fit: cover;
       object-fit: cover;
    -webkit-transition: var(--bg-transition);
    -o-transition: var(--bg-transition);
    transition: var(--bg-transition);
    opacity: 0.1;
    opacity: 0;
    -webkit-animation: fadeIn-vid 0.5s 0.4s forwards;
            animation: fadeIn-vid 0.5s 0.4s forwards;
}


.projects_wrapper {
    position: relative;
    scroll-snap-align: start;
    width: 100%;
    height: calc(45vw*v-bind(project_count));
    -webkit-box-shadow: 0px 20px 10px rgba(31, 31, 31, 0.096);
            box-shadow: 0px 20px 10px rgba(31, 31, 31, 0.096);
    -webkit-transition: var(--bg-transition) 0.5s;
    -o-transition: var(--bg-transition) 0.5s;
    transition: var(--bg-transition) 0.5s;
}

.project_bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: -webkit-gradient(linear, left bottom, left top, from(var(--background-color)), color-stop(5%, rgba(255,255,255,0)), color-stop(95%, rgba(255,255,255,0)), to(var(--background-color)));
    background: -o-linear-gradient(bottom, var(--background-color) 0%, rgba(255,255,255,0) 5%, rgba(255,255,255,0) 95%, var(--background-color) 100%);
    background: linear-gradient(0deg, var(--background-color) 0%, rgba(255,255,255,0) 5%, rgba(255,255,255,0) 95%, var(--background-color) 100%);
    opacity: 0;
    -webkit-animation: fadeIn 0.5s  0.4s forwards;
            animation: fadeIn 0.5s  0.4s forwards;
}

.item_wrapper {
    position: relative;
    z-index: 3;
    width: 100%;
    height: 45vw;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    padding: 5vw  0;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
}

/* shift all even 20% right and all odd 20% left */
.item_wrapper:nth-child(even) {
    -webkit-transform: translateX(4%);
        -ms-transform: translateX(4%);
            transform: translateX(4%);
}

.item_wrapper:nth-child(odd) {
    -webkit-transform: translateX(-12%);
        -ms-transform: translateX(-12%);
            transform: translateX(-12%);
}

@-webkit-keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@-webkit-keyframes fadeIn-vid {
    from {
        opacity: 0;
    }
    to {
        opacity: 0.1;
    }
}

@keyframes fadeIn-vid {
    from {
        opacity: 0;
    }
    to {
        opacity: 0.1;
    }
}

@media only screen and (max-width: 768px) {
    .item_wrapper {
        height: 80svh;
    }
    .projects_wrapper {
        height: calc(80svh*v-bind(project_count));
    }
    .item_wrapper:nth-child(even) {
        -webkit-transform: translateX(-7%);
            -ms-transform: translateX(-7%);
                transform: translateX(-7%);
        -webkit-box-pack: right;
            -ms-flex-pack: right;
                justify-content: right;
    }

    .item_wrapper:nth-child(odd) {
        -webkit-transform: translateX(0%);
            -ms-transform: translateX(0%);
                transform: translateX(0%);
        -webkit-box-pack: left;
            -ms-flex-pack: left;
                justify-content: left;
    }
}
</style>