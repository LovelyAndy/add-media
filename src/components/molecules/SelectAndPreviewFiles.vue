<template>
  <div class="select-and-preview-files">
    <div v-if="selectedFiles">
      <div :key="index" v-for="(selectedFile, index) in selectedFiles">
        <img :src="selectedFile" alt="" />
        <button @click="deleteFile(index)">Delete</button>
      </div>
    </div>

    <UploadMediaFiles
      v-model="selectedFiles"
      @selected="(files) => selectFiles(files)"
      v-slot="{ openFileDialog }"
      :multiple="true"
    >
      <button @click="openFileDialog">CLICK ME</button>
    </UploadMediaFiles>
  </div>
</template>

<style lang="sass" scoped>
// $

// .select-and-preview-files
img
  width: 20%
  margin: auto
  display: block
  margin-bottom: 10px
</style>

<script>
import UploadMediaFiles from '../atoms/UploadMediaFiles.vue'
export default {
  name: 'SelectAndPreviewFiles',
  components: {
    UploadMediaFiles,
  },
  props: {},
  data() {
    return {
      selectedFiles: [],
    }
  },
  computed: {},
  methods: {
    selectFiles(files) {
      let self = this
      for (let index = 0; index < files.length; index++) {
        const file = files[index]
        let reader = new FileReader()
        reader.onload = function (event) {
          const imageUrl = event.target.result
          self.selectedFiles.push(imageUrl)
        }
        reader.readAsDataURL(file)
      }
      this.$emit('input', this.selectedFiles)
    },
    deleteFile(index) {
      this.$emit('input', this.selectedFiles.splice(index, 1))
    },
  },
}
</script>
