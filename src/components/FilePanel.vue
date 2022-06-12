<template>
  <div class="panel">
    <div class="header" :title="directory">{{ fullPath }}</div>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Modified</th>
          <th>Size</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td colspan="3" center>
            <a href="#" @click.prevent="chdir('..')">..</a>
          </td>
        </tr>
        <tr v-for="file in files" :key="file.name">
          <template v-if="file.isDirectory">
            <td>
              [<a href="#" @click.prevent="chdir(file.name)">{{ file.name }}</a
              >]
            </td>
            <td>{{ lastAccessTime(file.ctime) }}</td>
            <td>n/a</td>
          </template>
          <template v-else>
            <td>{{ file.name }}</td>
            <td>{{ lastAccessTime(file.ctime) }}</td>
            <td>{{ file.size }}</td>
          </template>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import { readdir, stat } from "fs/promises";
import { watch } from "fs";
import path from "path";
import { ref } from "vue";
export default {
  props: {
    directory: {
      type: String,
      required: true,
    },
  },
  setup(props, { emit }) {
    const lastAccessTime = (ms) => {
      return new Date(ms).toLocaleDateString();
    };
    const fullPath = ref(path.resolve(props.directory));
    const files = ref([]);
    const scan = () => {
      readdir(props.directory).then((list) => {
        Promise.all(
          list.map((name) => {
            const fullPath = path.resolve(props.directory, name);
            return stat(fullPath)
              .then((stats) => {
                const isDirectory = stats.isDirectory();
                return { name, ...stats, fullPath, isDirectory };
              })
              .catch((error) => {
                console.log(fullPath);
                console.error(error);
              });
          })
        ).then((list) => {
          files.value = list.sort((a, b) => {
            if (a.isDirectory === b.isDirectory) {
              return a.name.localeCompare(b);
            }
            return b.isDirectory - a.isDirectory;
          });
        });
      });
    };
    const chdir = (dir) => {
      emit("chdir", path.resolve(props.directory, dir));
    };
    scan();
    watch(props.directory, scan);
    return { lastAccessTime, files, fullPath, chdir };
  },
};
</script>
