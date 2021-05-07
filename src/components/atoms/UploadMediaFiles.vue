<template>
  <div class="upload-media-files">
    <input
      style="display: none"
      id="input-file"
      type="file"
      accept="*"
      multiple
      @change="addMedia"
      class="_add-media-input"
      ref="input"
    />
    <label for="input-file">
      <slot :openFileDialog="openFileDialog">
        <img
          src="https://www.clipartmax.com/png/middle/142-1422132_png-file-svg-upload-file-icon-png.png"
          alt=""
        />
      </slot>
    </label>
  </div>
</template>

<style lang="sass" scoped>
// $
// .upload-media-files
input
  display: none
</style>

<script>
export default {
  name: 'UploadMediaFiles',
  props: {
    multiple: { type: Boolean },
    accept: { type: String },
  },
  data() {
    return {
      // files: [],
    }
  },
  computed: {},
  methods: {
    async addMedia(event) {
      const files = event.target.files || event.dataTransfer.files
      // if (!files.length) return
      // console.log(`files → `, files)
      // this.files.push(files)
      this.$emit('selected', files)
    },

    openFileDialog() {
      this.$refs.input.click()
    },
  },
}
</script>
