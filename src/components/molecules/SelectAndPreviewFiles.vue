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
    <button class="_btn" @click="share">1. Share w/ Text</button>
    <button class="_btn" @click="basicShare">2. Basic Share</button>
    <button class="_btn" @click="shareImgURL">3. Share Image URL</button>
    <button class="_btn" @click="shareImg">4. Share Image</button>
    <button class="_btn" @click="shareImgLinkAndText">5. Share Image and Text</button>
    <button class="_btn" @click="shareCapturedImg">6. Share Captured Image</button>
  </div>
</template>

<style lang="sass" scoped>
.select-and-preview-files
  display: flex
  flex-direction: column
  justify-content: space-around
  height: 100vh
._btn
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
    /////// Upload Media Stuff ^^^^^^
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
    async shareImgURL() {
      await Share.share({
        url: 'https://floodmagazine.com/wp-content/uploads/2016/07/Steve_Brule-2016-Marc_Lemoine-7-746x1024.jpg',
      })
    },
    async shareImg() {
      const urlToFile = async (url) => {
        const response = await fetch(url)
        const blob = response.blob()
        const file = new File([blob], 'image.jpg', { type: blob.type })
        return file
      }
      if (window.navigator.share) {
        const file = await urlToFile(this.selectedFiles)
        const files = [file]
        if (navigator.canShare || !navigator.canShare({ files: files })) {
          alert('Can use this feature')
          return
        }
        Share.share({
          files: files,
          title: 'shared image',
        })
          .then(() => console.log('you did it!'))
          .catch((error) => console.log(`file error → `, error))
      } else alert('fuck')
    },
    async shareImgLinkAndText() {
      await Share.share({
        title: 'A Nice Web App',
        text: 'WHAT A BEAUTIFUL WEBAPP',
        url: 'https://weather-boyy.web.app/',
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
