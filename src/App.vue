<template>
  <FilePanel :directory="dir" @chdir="chdir" :key="dir" />
  <NewWin />
</template>

<script>
import FilePanel from "./components/FilePanel.vue";
import NewWin from "./components/NewWin.vue";
export default {
  name: "App",
  components: {
    FilePanel,
    NewWin,
  },
  data() {
    return {
      dir: ".",
    };
  },
  methods: {
    chdir(path) {
      this.dir = path;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}
</style>
