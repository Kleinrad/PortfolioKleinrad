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
  font-size: 1.2em;
  margin: 0vw 2.5vw;
}

.header_language > span {
  margin: 0 0.5vw;
  cursor: default;
}

.not_selected {
  opacity: 0.4;
  cursor: pointer !important;
}

</style>