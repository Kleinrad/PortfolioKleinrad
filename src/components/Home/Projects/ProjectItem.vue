<template>
    <div id="pItem" :class="{'project_item':true, 'project_item_active': fullSize}">
        <span id="cover" :style="{'mix-blend-mode': project.media.includes('webp')?'luminosity':'color'}"></span>
        <img id="cover" :class="{'image_background' : project.media.includes('webp') && fullSize}" v-if="project.media.includes('webp') && fullSize && screenType != 0" :src="project.media" alt="project image">
        <iframe
            v-if="fullSize && project.media.includes('youtube') && screenType != 0"
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
        "screenType": Number,
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
    -webkit-animation: fadeIn 0.5s 1.8s forwards;
            animation: fadeIn 0.5s 1.8s forwards;
    -webkit-filter: brightness(0.3) saturate(0.85);
            filter: brightness(0.3) saturate(0.85);
}
.project_item {
    position: relative;
    width: 40vw;
    height: 20vw;
    overflow: hidden;
    cursor: pointer;
    background-color: var(--secondary-color);
    -webkit-transition: 0.5s;
    -o-transition: 0.5s;
    transition: 0.5s;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    -webkit-animation: morph-size-reverse 2s forwards;
            animation: morph-size-reverse 2s forwards;
}

.project_content {
    position: relative;
    z-index: 1;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    padding: 2vw;
    width: 40vw;
    height: 20vw;
    margin-top: 3vw;
    border-radius: 10%;
    background-color: rgba(16, 16, 16, 0.531);
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-pack: justify;
        -ms-flex-pack: justify;
            justify-content: space-between;
    mix-blend-mode: normal;
    cursor: default;
    opacity: 0;
    -webkit-animation: fadeIn 0.5s 3s forwards;
            animation: fadeIn 0.5s 3s forwards;
}

.project_item_active {
    width: v-bind(maxWidth);
    height: v-bind(maxHeight);
    -webkit-transition: width 0.5s ease-in-out, height 0.5s ease-in-out, -webkit-box-shadow 1s 0.5s ease-in-out;
    transition: width 0.5s ease-in-out, height 0.5s ease-in-out, -webkit-box-shadow 1s 0.5s ease-in-out;
    -o-transition: width 0.5s ease-in-out, height 0.5s ease-in-out, box-shadow 1s 0.5s ease-in-out;
    transition: width 0.5s ease-in-out, height 0.5s ease-in-out, box-shadow 1s 0.5s ease-in-out;
    transition: width 0.5s ease-in-out, height 0.5s ease-in-out, box-shadow 1s 0.5s ease-in-out, -webkit-box-shadow 1s 0.5s ease-in-out;
    -webkit-animation: morph-size 2s forwards, gooey 20s 2s infinite !important;
            animation: morph-size 2s forwards, gooey 20s 2s infinite !important;
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
    -webkit-animation: slide-up 0.8s 2s forwards cubic-bezier( 0.17, 0, 0.38, 1.01 );
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
    -webkit-animation: center-expand 0.5s 2.6s forwards ease-out;
            animation: center-expand 0.5s 2.6s forwards ease-out;
}

.project_technologies {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
        flex-wrap: wrap;
    background-color: rgba(105, 105, 105, 0.582);
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    padding: 0.5vw;
    border-radius: 5%;
    -webkit-box-pack: space-evenly;
        -ms-flex-pack: space-evenly;
            justify-content: space-evenly;
}


.project_links {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
        flex-wrap: wrap;
    -webkit-box-pack: space-evenly;
        -ms-flex-pack: space-evenly;
            justify-content: space-evenly;
}

.project_text {
    -webkit-box-sizing: border-box;
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
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    z-index: 0;
    background-color: var(--secondary-color);
    cursor: default;
    mix-blend-mode: luminosity;
}

.project_video {
    position: absolute;
    left: 50%;
    top: 50%;
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    width: 100%;
    height: 135%;
    z-index: -1;
    opacity: 0;
    -webkit-animation: fadeIn 0.5s 1.8s forwards;
            animation: fadeIn 0.5s 1.8s forwards;
    -webkit-filter: brightness(0.3) saturate(0.85);
            filter: brightness(0.3) saturate(0.85);
}


@-webkit-keyframes gooey {
  0%, 100% {
    border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%; /* Set the initial and final border radius */
  }
  10%, 90% {
    border-radius: 30% 40% 10% 5% / 5% 10% 40% 30%; /* Add some variation to the border radius */
  }
  20%, 80% {
    border-radius: 40% 50% 5% 10% / 10% 5% 50% 40%; /* Add some variation to the border radius */
  }
  30%, 70% {
    border-radius: 50% 40% 10% 15% / 15% 10% 60% 50%; /* Add some variation to the border radius */
  }
  40%, 60% {
    border-radius: 30% 30% 15% 20% / 50% 20% 10% 15%; /* Add some variation to the border radius */
  }
  50% {
    border-radius: 40% 50% 20% 25% / 25% 20% 50% 40%; /* Add some variation to the border radius */
  }
}


@keyframes gooey {
  0%, 100% {
    border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%; /* Set the initial and final border radius */
  }
  10%, 90% {
    border-radius: 30% 40% 10% 5% / 5% 10% 40% 30%; /* Add some variation to the border radius */
  }
  20%, 80% {
    border-radius: 40% 50% 5% 10% / 10% 5% 50% 40%; /* Add some variation to the border radius */
  }
  30%, 70% {
    border-radius: 50% 40% 10% 15% / 15% 10% 60% 50%; /* Add some variation to the border radius */
  }
  40%, 60% {
    border-radius: 30% 30% 15% 20% / 50% 20% 10% 15%; /* Add some variation to the border radius */
  }
  50% {
    border-radius: 40% 50% 20% 25% / 25% 20% 50% 40%; /* Add some variation to the border radius */
  }
}

@-webkit-keyframes morph-size{
    0%{
        border-radius: 0%;
        -webkit-transform: scale(1);
                transform: scale(1);
    }
    20%{
        border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
        -webkit-transform: scale(0.5);
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
        -webkit-transform: scale(1);
                transform: scale(1);
    }
}

@keyframes morph-size{
    0%{
        border-radius: 0%;
        -webkit-transform: scale(1);
                transform: scale(1);
    }
    20%{
        border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
        -webkit-transform: scale(0.5);
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
        -webkit-transform: scale(1);
                transform: scale(1);
    }
}

@-webkit-keyframes morph-size-reverse{
    0%{
        border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%;
        -webkit-transform: scale(1);
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
        -webkit-transform: scale(0.8);
                transform: scale(0.8);
    }
    100%{
        border-radius: 0%;
        -webkit-transform: scale(1);
                transform: scale(1);
    }
}

@keyframes morph-size-reverse{
    0%{
        border-radius: 50% 30% 20% 10% / 10% 20% 30% 50%;
        -webkit-transform: scale(1);
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
        -webkit-transform: scale(0.8);
                transform: scale(0.8);
    }
    100%{
        border-radius: 0%;
        -webkit-transform: scale(1);
                transform: scale(1);
    }
}

@-webkit-keyframes fadeIn {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
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

@-webkit-keyframes slide-up {
    0% {
        -webkit-transform: translateY(0%);
                transform: translateY(0%);
    }
    100% {
        -webkit-transform: translateY(-300%);
                transform: translateY(-300%);
    }
}

@keyframes slide-up {
    0% {
        -webkit-transform: translateY(0%);
                transform: translateY(0%);
    }
    100% {
        -webkit-transform: translateY(-300%);
                transform: translateY(-300%);
    }
}

@-webkit-keyframes center-expand {
    0% {
        width: 0%;
        -webkit-transform: translateX(-50%);
                transform: translateX(-50%)
    }
    100% {
        width: 130%;
        -webkit-transform: translateX(-50%);
                transform: translateX(-50%)
    }
}

@keyframes center-expand {
    0% {
        width: 0%;
        -webkit-transform: translateX(-50%);
                transform: translateX(-50%)
    }
    100% {
        width: 130%;
        -webkit-transform: translateX(-50%);
                transform: translateX(-50%)
    }
}

@media screen and (max-width: 768px) {
    .project_item {
        height: 50%;
        width: 60%;
        opacity: 0.9;
    }
    .project_item_active{
        height: 85%;
        width: 80%;
    }
    .project_content {
        width: 90%;
        height: 80%;
        padding: 6%;
        margin-top: 15vw;
        -webkit-box-pack: space-evenly;
            -ms-flex-pack: space-evenly;
                justify-content: space-evenly;
    }

    .project_name{
        font-size: 5vw;
    }

    .project_name_active{
        font-size: 7vw !important;
    }

    .project_text {
        font-size: 4vw;
        line-height: 4.6vw;
    }

    @-webkit-keyframes slide-up {
        0% {
            -webkit-transform: translateY(0%);
                    transform: translateY(0%);
        }
        100% {
            -webkit-transform: translateY(-600%);
                    transform: translateY(-600%);
        }
    }

    @keyframes slide-up {
        0% {
            -webkit-transform: translateY(0%);
                    transform: translateY(0%);
        }
        100% {
            -webkit-transform: translateY(-600%);
                    transform: translateY(-600%);
        }
    }

    @-webkit-keyframes gooey {
        0%, 100% {
            border-radius: 10% 6% 4% 2% / 2% 4% 6% 10%; /* Set the initial and final border radius */
        }
        10%, 90% {
            border-radius: 6% 8% 2% 1% / 1% 2% 8% 6%; /* Add some variation to the border radius */
        }
        20%, 80% {
            border-radius: 8% 10% 1% 2% / 2% 1% 10% 8%; /* Add some variation to the border radius */
        }
        30%, 70% {
            border-radius: 10% 8% 2% 3% / 3% 2% 12% 10%; /* Add some variation to the border radius */
        }
        40%, 60% {
            border-radius: 6% 6% 3% 4% / 10% 4% 2% 3%; /* Add some variation to the border radius */
        }
        50% {
            border-radius: 8% 10% 4% 5% / 5% 4% 10% 8%; /* Add some variation to the border radius */
        }
    }

    @keyframes gooey {
        0%, 100% {
            border-radius: 10% 6% 4% 2% / 2% 4% 6% 10%; /* Set the initial and final border radius */
        }
        10%, 90% {
            border-radius: 6% 8% 2% 1% / 1% 2% 8% 6%; /* Add some variation to the border radius */
        }
        20%, 80% {
            border-radius: 8% 10% 1% 2% / 2% 1% 10% 8%; /* Add some variation to the border radius */
        }
        30%, 70% {
            border-radius: 10% 8% 2% 3% / 3% 2% 12% 10%; /* Add some variation to the border radius */
        }
        40%, 60% {
            border-radius: 6% 6% 3% 4% / 10% 4% 2% 3%; /* Add some variation to the border radius */
        }
        50% {
            border-radius: 8% 10% 4% 5% / 5% 4% 10% 8%; /* Add some variation to the border radius */
        }
    }

    @-webkit-keyframes morph-size{
        0%{
            border-radius: 0%;
            -webkit-transform: scale(1);
                    transform: scale(1);
        }
        20%{
            border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
            -webkit-transform: scale(0.5);
                    transform: scale(0.5);
        }
        35%{
            border-radius: 40% 36% 43% 34% / 40% 36% 43% 34%;
        }
        60%{
            border-radius: 20% 18% 16% 7% / 20% 18% 16% 7%;
        }
        100%{
            border-radius: 10% 6% 4% 2% / 2% 4% 6% 10%;
            -webkit-transform: scale(1);
                    transform: scale(1);
        }
    }

    @keyframes morph-size{
        0%{
            border-radius: 0%;
            -webkit-transform: scale(1);
                    transform: scale(1);
        }
        20%{
            border-radius: 40% 35% 41% 33% / 40% 35% 41% 33%;
            -webkit-transform: scale(0.5);
                    transform: scale(0.5);
        }
        35%{
            border-radius: 40% 36% 43% 34% / 40% 36% 43% 34%;
        }
        60%{
            border-radius: 20% 18% 16% 7% / 20% 18% 16% 7%;
        }
        100%{
            border-radius: 10% 6% 4% 2% / 2% 4% 6% 10%;
            -webkit-transform: scale(1);
                    transform: scale(1);
        }
    }
}
</style>