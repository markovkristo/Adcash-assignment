<template>
  <div id="app">
      <TextArea/>
  </div>
</template>

<script>
import TextArea from "./components/TextArea.vue";

export default {
  name: "App",
  components: {
    TextArea,
  },
};
</script>

<style lang="scss">
@import "assets/variables.scss";
#app {
  font-family: $fontFamily;
  font-size: $fontSize;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
  width: $width;
}

body {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-bottom: 40px;
}
</style>
