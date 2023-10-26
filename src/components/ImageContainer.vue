<template>
  <h1>Image Reader</h1>

  <div
    class="dropzone"
    @dragover.prevent
    @dragenter.prevent
    @dragstart.prevent
    @drop.prevent="handleFileChange($event.dataTransfer)"
  >
    <input
      id="file-input"
      type="file"
      accept="image/png, image/jpeg"
      @change="handleFileChange($event.target)"
      required
    />
    <h2 v-if="!preview" for="file-input">Click or Drag N Drop Image</h2>
    <img v-bind:src="preview" />
  </div>

  <div class="button-container">
    <button type="button" v-on:click="read">Read</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      preview: null,
      formData: null,
      cloudName: "",
    };
  },
  methods: {
    handleFileChange: function (event) {
      this.file = event.files[0];

      this.formData = new FormData();

      let reader = new FileReader();
      reader.readAsDataURL(this.file);

      reader.onload = (e) => {
        this.preview = e.target.result;
        this.formData.append("file", this.preview);
      };
    },
    read: async function () {
      this.preview = null;
      this.formData = null;
    },
  },
};
</script>