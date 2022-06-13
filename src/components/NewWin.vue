<template>
  <button @click="newWin">Open new window</button>
</template>

<script>
import { ipcRenderer } from "electron";
export default {
  name: "NewWin",
  methods: {
    newWin() {
      ipcRenderer.send("open");
    },
  },
};
</script>
