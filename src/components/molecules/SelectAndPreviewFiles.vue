<template>
  <div class="select-and-preview-files">
    <div v-if="selectedFiles">
      <div :key="index" v-for="(selectedFile, index) in selectedFiles">
        <img v-if="selectedFile.startsWith('data:image')" :src="selectedFile" alt="" />
        <video v-if="selectedFile.startsWith('data:video')" controls :src="selectedFile" alt="" />
        <button @click="deleteFile(index)">Delete</button>
      </div>
    </div>

    <UploadMediaFiles
      @selected="selectFiles"
      v-slot="{ openFileDialog }"
      :multiple="true"
      :accept="'*'"
    >
      <button @click="openFileDialog">CLICK ME</button>
    </UploadMediaFiles>
    <button class="btn" @click="share">share</button>
  </div>
</template>

<style lang="sass" scoped>
.select-and-preview-files
  display: flex
  flex-direction: column
  justify-content: space-around
  height: 100vh
.btn
  width: 50%
  align-self: center
  color: white
  background-color: indianred
img
  width: 20%
  margin: auto
  display: block
  margin-bottom: 10px
</style>

<script>
import UploadMediaFiles from '../atoms/UploadMediaFiles.vue'
import { Plugins } from '@capacitor/core'
const { Share } = Plugins

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
      let promises = []
      for (let index = 0; index < files.length; index++) {
        let promise = new Promise((resolve, reject) => {
          const file = files[index]
          let reader = new FileReader()
          reader.onload = function (event) {
            const url = event.target.result
            self.selectedFiles.push(url)
            resolve()
          }
          reader.readAsDataURL(file)
        })
        promises.push(promise)
      }
      Promise.all(promises).then(() => self.$emit('input', self.selectedFiles))
    },
    deleteFile(index) {
      this.$emit('input', this.selectedFiles.splice(index, 1))
    },
    async share() {
      let shareRet = await Share.share({
        title: 'See cool stuff',
        text: 'Really awesome thing you need to see right meow',
        url: 'http://ionicframework.com/',
        dialogTitle: 'Share with buddies',
      })
    },
  },
}
</script>
