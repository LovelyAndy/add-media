<template>
  <div class="select-and-preview-files">
    <div v-if="selectedFiles">
      <div :key="index" v-for="(selectedFile, index) in selectedFiles">
        <img :src="selectedFile" alt="" />
        <button @click="deleteFile(index)">Delete</button>
      </div>
    </div>
    <!-- <img />
      //OR
      <video /> -->
    <!-- <img :src="selectedFile" alt="" />-->
    <UploadMediaFiles @selected="(files) => selectFiles(files)" v-slot="{ openFileDialog }">
      <button @click="openFileDialog">
        <img
          src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a1/Circle-icons-upload.svg/1200px-Circle-icons-upload.svg.png"
          alt=""
        />
      </button>
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
      this.selectedFiles.push(files)
      this.previewImage(files)
    },
    previewImage(files) {
      var vm = this
      for (var index = 0; index < files.length; index++) {
        const fileList = files[index]
        console.log(`fileList → `, fileList)
        fileList.forEach((file) => {
          var reader = new FileReader()
          reader.onload = function (event) {
            const imageUrl = event.target.result
            vm.selectedFiles.push(imageUrl)
          }
          reader.readAsDataURL(file)
        })
      }
    },
    deleteFile(index) {
      this.selectedFiles.splice(index, 1)
    },
  },
}
</script>
