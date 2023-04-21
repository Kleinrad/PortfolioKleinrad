<template>
    <div class="about_text_wrapper">
        <div class="split_text_container">
            <p>Hello, this is a test</p>
            <p>How does it look</p>
        </div>
    </div>
    <svg id="filters">
        <defs>
            <filter id="threshold">
                <feColorMatrix in="SourceGraphic" type="matrix" values="1 0 0 0 0
                                        0 1 0 0 0
                                        0 0 1 0 0
                                        0 0 0 255 -140" />
            </filter>
        </defs>
    </svg>
</template>

<script>

</script>

<style scoped>
.about_text_wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
}

.split_text_container {
    position: absolute;
    top: 20%;
    left: 50%;
    width: 200px;
    transform: translate(-50%, -50%);
    z-index: 1;
    filter: url(#threshold) blur(0.5px);
}


.split_text_container p {
    position: absolute;
    font-size: 4vw;
}

.split_text_container p:nth-child(1) {
    opacity: 1;
    animation: blurOut 1s ease-in 2s forwards;
}

.split_text_container p:nth-child(2) {
    opacity: 0;
    animation: blurIn 1s ease-out 2s forwards;
}

@keyframes blurOut {
    0% {
        filter: blur(0px);
        opacity: 1;
    }
    100% {
        filter: blur(8px);
        opacity: 0;
    }
}

@keyframes blurIn {
    0% {
        opacity: 0;
        filter: blur(8px);
    }
    100% {
        filter: blur(0px);
        opacity: 1;
    }
}
</style>