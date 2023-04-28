<template>
    <div class="banner_carousel">
        <div class="carousel_container" >
            <span v-for="text in this.textArray">{{text}}</span>
        </div>
        <div class="carousel_container" >
            <span v-for="text in this.textArray">{{text}}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: "HomeCarousel",
    data() {
        return {
            textArray: ["design", "coding", "create-new"],
        };
    },
    mounted() {
        //insert empty string to create space between words
        let arr_copy = this.textArray.slice();
        for (let i = 0; i < this.textArray.length - 1; i++) {
            arr_copy.splice(i*2 + 1, 0, "");
        }
        this.textArray = arr_copy;
    },
};
</script>

<style scoped>
.banner_carousel {
    position: relative;
    width: 100vw;
    height: 20svh;
}
.carousel_container {
    position: absolute;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: space-evenly;
        -ms-flex-pack: space-evenly;
            justify-content: space-evenly;
    width: 100vw;
    font-size: 5vw;
    font-weight: 1000;
    font-style: italic;
    overflow-x: hidden;
    color: #fff;
    /* outline text in black*/
    color: black;
    -webkit-text-fill-color: white; /* Will override color (regardless of order) */
    -webkit-text-stroke-width: 1px;
    -webkit-text-stroke-color: black;
}

.carousel_container:first-child {
    -webkit-transform: translate(0, 0);
        -ms-transform: translate(0, 0);
            transform: translate(0, 0);
    -webkit-animation: carousel_first 15s linear infinite;
            animation: carousel_first 15s linear infinite;
}

.carousel_container:last-child {
    -webkit-transform: translate(100%, 0);
        -ms-transform: translate(100%, 0);
            transform: translate(100%, 0);
    -webkit-animation: carousel_second 15s linear infinite;
            animation: carousel_second 15s linear infinite;
}
.carousel_container > span{ 
    -webkit-transition: 0.5s ease-in-out; 
    -o-transition: 0.5s ease-in-out; 
    transition: 0.5s ease-in-out; 
    cursor: default;
}

.carousel_container > span:hover{
    -webkit-transform: translateY(-1svh);
        -ms-transform: translateY(-1svh);
            transform: translateY(-1svh);
    text-shadow: 0px 1svh 0px var(--accent-color);
    -webkit-transition: 0.5s ease-in-out;
    -o-transition: 0.5s ease-in-out;
    transition: 0.5s ease-in-out;
}

@-webkit-keyframes carousel_first {
    0% {
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }
    100% {
        -webkit-transform: translate(-100%, 0);
                transform: translate(-100%, 0);
    } 
}

@keyframes carousel_first {
    0% {
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    }
    100% {
        -webkit-transform: translate(-100%, 0);
                transform: translate(-100%, 0);
    } 
}

@-webkit-keyframes carousel_second {
    0% {
        -webkit-transform: translate(100%, 0);
                transform: translate(100%, 0);
    }
    100% {
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    } 
}

@keyframes carousel_second {
    0% {
        -webkit-transform: translate(100%, 0);
                transform: translate(100%, 0);
    }
    100% {
        -webkit-transform: translate(0, 0);
                transform: translate(0, 0);
    } 
}

@media screen and (max-width: 768px) {
    .carousel_container {
        font-size: 10vw;
        width: 200vw;
    }

    .carousel_container > span{
        margin: 0vw 3vw;
    }

    .carousel_container:first-child {
        -webkit-animation: carousel_first 10s linear infinite;
                animation: carousel_first 10s linear infinite;
    }

    .carousel_container:last-child {
        -webkit-animation: carousel_second 10s linear infinite;
                animation: carousel_second 10s linear infinite;
    }

}
</style>