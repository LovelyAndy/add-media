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
    <button class="btn1" @click="share">Share w/ Text</button>
    <button class="btn2" @click="basicShare">Basic Share</button>
    <button class="btn3" @click="shareImg">Share Image</button>
    <button class="btn4" @click="shareCapturedImg">Share Captured Image</button>
    <button class="btn5" @click="shareImgAndText">Share Image and Text</button>
  </div>
</template>

<style lang="sass" scoped>
.select-and-preview-files
  display: flex
  flex-direction: column
  justify-content: space-around
  height: 100vh
.btn1,
.btn2,
.btn3,
.btn4,
.btn5
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
import { CameraResultType, CameraSource, Plugins, registerWebPlugin } from '@capacitor/core'
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
      await Share.share({
        title: 'A Nice Web App',
        text: 'WHAT A BEAUTIFUL WEBAPP',
        url: 'https://weather-boyy.web.app/',
        dialogTitle: 'Share with buddies', // android only
      })
    },
    async basicShare() {
      await Share.share({
        url: 'https://www.youtube.com/watch?v=HWZFqEXH5a0',
      })
    },
    async shareImg() {
      await Share.share({
        url: 'https://floodmagazine.com/wp-content/uploads/2016/07/Steve_Brule-2016-Marc_Lemoine-7-746x1024.jpg',
      })
    },
    async shareCapturedImg() {
      const image = await Plugins.Camera.getPhoto({
        quality: 100,
        allowEditing: false,
        resultType: CameraResultType.Uri,
        source: CameraSource.Camera,
      })
      await Share.share({
        title: 'new profile picture',
        url: image.path,
      })
    },
  },
}
</script>
