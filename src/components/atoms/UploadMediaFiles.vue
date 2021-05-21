<template>
  <div class="upload-media-files">
    <input
      style="display: none"
      id="input-file"
      type="file"
      :accept="accept"
      :multiple="multiple"
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
input
  display: none
</style>

<script>
export default {
  name: 'UploadMediaFiles',
  props: {
    multiple: { type: Boolean, default: false },
    accept: { type: String, default: '*' },
  },
  data() {
    return {}
  },
  computed: {},
  methods: {
    addMedia(event) {
      const files = event.target.files || event.dataTransfer.files
      this.$emit('selected', files)
      console.log(`files → `, files)
    },

    openFileDialog() {
      this.$refs.input.click()
    },
  },
}
</script>
