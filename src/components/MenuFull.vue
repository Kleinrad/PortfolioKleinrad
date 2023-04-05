<template>
    <div class="menu_wrapper">
        <div class="menu_inner">
            <span>projects</span>
            <span>technologies</span>
            <span>about me</span>
        </div>
        <div class="menu_footer">
            <span>Say Hello (:</span>
            <span>fabiankleinrad.fk@gmail.com</span>
            <div class="socials">placeholder</div>
        </div>
    </div>
</template>

<script>

</script>

<style scoped>
.menu_wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100svh;
    background-color: #fff;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    padding-left: 5vw;
    box-sizing: border-box;
    animation: fadeIn .5s forwards;
}

.menu_inner {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 4em;
    font-weight: 1000;
    height: 50vh;
    text-align: left;
    margin: auto;
    margin-left: 0;
}

.menu_inner > span {
    opacity: 0.3;
    height: 95px;
    animation: slide-together .15s forwards;
}

.menu_inner > span:nth-child(1) { animation-delay: 0.1s; }
.menu_inner > span:nth-child(2) { animation-delay: 0.15s; }
.menu_inner > span:nth-child(3) { animation-delay: 0.2s; }


.menu_inner > span {
    cursor: pointer;
    transition: 0.3s ease-in-out;
}

.menu_inner > span:hover {
    margin: 0 1vw;
    color: var(--primary-color);
    transition: transform 0.35s, color 0.3 cubic-bezier(.39,.575,.565,1);
}

.menu_footer {
    display: flex;
    width: 100%;
    margin-bottom: 10svh;
}

.menu_footer > div {
    display: inline;
    margin-left: auto;
    margin-right: 5vw;
}

@keyframes slide-together {
    100% {
        height: 80px;
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
</style>