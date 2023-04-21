<template>
    <div class="header_language">
        <span :class="[en_selected ? 'not_selected': '']" @click="en_selected = false">de</span>
        <span :class="[!en_selected ? 'not_selected' : '']" @click="en_selected = true">en</span>
    </div>
</template>

<script>
export default {
  name: "HeaderLang",
  data() {
    return {
      en_selected: false,
    };
  },
};
</script>

<style scoped>
.header_language {
  display: flex;
  align-items: flex-end;
  min-width: fit-content;
  font-size: 1.5vw;
  margin: 0vw 2.5vw;
}

.header_language > span {
  margin: 0 0.5vw;
  cursor: default;
}

.header_language > span:hover {
  opacity: 0.8;
}

.not_selected {
  opacity: 0.4;
  cursor: pointer !important;
  transition: 0.1s;
}

@media screen and (max-width: 768px) {
  .header_language {
    margin: 0 5vw;
    font-size: 4.5vw;
  }

  .header_language > span {
    margin: 0 2vw;
  }
}

@media screen and (max-width: 1050px) {
  .header_language {
    margin: 0 2.5vw;
    font-size: 2vw;
  }

  .header_language > span {
    margin: 0 1vw;
  }
}

</style>