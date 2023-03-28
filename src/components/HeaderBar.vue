<template>
  <header class="header">
    <div class="header_inner">
      <div class="header_logo">
        <span class="header_logo_text">#Demosite</span>
      </div>
      <div class="header_nav">
        <span>projects</span>
        <span>technologies</span>
        <span>about me</span>
      </div>
      <div class="header_language">
        <span>de</span>
        <span>en</span>
      </div>
      <div class="menu_btn" id="header_menu_btn">
        <div id="menu_default"></div>
        <div id="menu_focus"></div>
      </div>
    </div>
  </header>
</template>

<script></script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #fff;
  z-index: 100;
}

.header_inner {
  display: flex;
  align-items: center;
  height: 100%;
  padding: 3vw 5vw 1.2vw;
}

.header_nav {
  display: flex;
  justify-content: space-evenly;
  width: 50vw;
  min-width: fit-content;
  font-size: 1.2em;
}

.header_nav > span {
  margin: 0 1vw;
  display: flex;
  align-items: flex-end;
}

.header_language {
  display: flex;
  align-items: flex-end;
  min-width: fit-content;
  font-size: 1.2em;
}

.header_language > span {
  margin: 0 0.5vw;
}

.header_logo {
  margin-right: auto;
  font-weight: bolder;
  font-size: 1.5em;
}

.menu_btn {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 10px;
  width: 1.5em;
  margin-left: 2vw;
  border-top: 3px solid #000;
  border-bottom: 3px solid #000;
}

.menu_btn:hover {
  height: 15px;
  transition: 0.15s ease-out;
}
</style>
