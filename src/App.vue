<template>
  <div id="app">
    <SelectAndPreviewFiles />
  </div>
</template>

<script>
import SelectAndPreviewFiles from './components/molecules/SelectAndPreviewFiles.vue'

export default {
  components: { SelectAndPreviewFiles },
  name: 'App',
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
