<template>
    <div :class="{'menu_icons':true, 'dark':dark}">
        <a href="https://www.linkedin.com/in/fabian-kleinrad-27892522b/">
            <img id="linkedIn_menu" src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png">
        </a>
        <a href="https://www.linkedin.com/in/fabian-kleinrad-27892522b/">
            <img id="email_menu" src="https://upload.wikimedia.org/wikipedia/commons/7/7f/OOjs_UI_icon_message.svg">
        </a>
    </div>
</template>

<script>
export default {
    name: "SocialIcons",
    props: {
        "dark": Boolean,
    },
    data() {
        return {
        }
    },
    mounted() {
    },
    methods: {
    },
}
</script>

<style scoped>

.dark {
    -webkit-filter: invert(1) saturate(0) brightness(1.4) !important;
            filter: invert(1) saturate(0) brightness(1.4) !important;
}
.menu_icons {
    position: absolute;
    bottom: 1%;
    height: 2vw;
    width: 100%;
    -webkit-filter: saturate(0) brightness(.8);
            filter: saturate(0) brightness(.8);
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    padding-left: 3vw;
    -webkit-box-pack: left;
        -ms-flex-pack: left;
            justify-content: left;
}

.menu_icons a {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    opacity: 0.5;
    -webkit-transition: opacity 0.5s ease-in-out;
    -o-transition: opacity 0.5s ease-in-out;
    transition: opacity 0.5s ease-in-out;
}

.menu_icons a:hover {
    opacity: 1;
}

#linkedIn_menu {
    height: 100%;
    aspect-ratio: 1/1;
}

#email_menu {
    height: 100%;
    aspect-ratio: 2/1;
}

@media screen and (max-width: 1050px) {

    .menu_icons {
        height: 5vw;
        padding-left: 5vw;
    }
}

@media screen and (max-width: 768px) {

    .menu_icons {
        height: 12vw;
        -webkit-box-pack: center;
            -ms-flex-pack: center;
                justify-content: center;
    }
}
</style>