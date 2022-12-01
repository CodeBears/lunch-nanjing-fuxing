<template>
  <DrawLunch />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import DrawLunch from "./components/DrawLunch.vue";

@Options({
  components: {
    DrawLunch,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
:root {
  --slice-height: 100px;
  --background: #f00;
}
body {
  background-color: #eea9a9;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
