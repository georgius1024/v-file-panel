<template>
  <Suspense>
    <FilePanel :directory="dir" @chdir="chdir" :key="dir" />
  </Suspense>
</template>

<script>
import FilePanel from "./components/FilePanel.vue";
export default {
  name: "App",
  components: {
    FilePanel,
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
