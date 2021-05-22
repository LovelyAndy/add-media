<template>
  <div id="app">
    <SelectAndPreviewFiles v-model="selectedFiles" />
  </div>
</template>

<script>
import SelectAndPreviewFiles from './components/molecules/SelectAndPreviewFiles.vue'

export default {
  components: { SelectAndPreviewFiles },
  name: 'App',
  data() {
    return {
      selectedFiles: [],
    }
  },
  watch: {
    selectedFiles() {
      console.log(`this.selectedFiles inside of app→ `, this.selectedFiles)
    },
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
