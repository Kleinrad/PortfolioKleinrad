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
        <span id="menu_default"></span>
        <span id="menu_focus"></span>
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
  background-color: #000;
  z-index: 100;
}

.header_inner {
  display: flex;
  justify-content: right;
  height: 100%;
  padding: 3vw 5vw 1.2vw;
}
</style>
