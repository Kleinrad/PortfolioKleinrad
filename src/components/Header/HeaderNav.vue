<template>
    <div class="header_nav">
      <span>projects</span>
      <span>technologies</span>
      <span>about me</span>
    </div>
</template>

<script>

</script>

<style scoped>
.header_nav {
  display: flex;
  justify-content: space-evenly;
  width: 30vw;
  min-width: fit-content;
  font-size: 1.2em;
}

.header_nav > span {
  margin: 0 1vw;
  display: flex;
  align-items: flex-end;
  cursor: pointer;
}

</style>