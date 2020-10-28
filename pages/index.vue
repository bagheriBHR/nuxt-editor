<template>
  <div class="container mt-5">
    <vue-editor v-model="content" useCustomImageHandler @image-added="handleImageAdded"></vue-editor>
    <p v-html="content"></p>
    <img :src="url" >
  </div>

</template>

<script>
  import axios from "axios";
  export default {

    data() {
      return {
        content: "<h1>Some initial content</h1>",
        url:''
      };
    },
    methods: {
      handleImageAdded: function(file, Editor, cursorLocation, resetUploader) {

        var formData = new FormData();
        formData.append("upload", file);

        axios({
          url: "http://127.0.0.1:8000/api/v1/ckUpload",
          method: "POST",
          data: formData
        })
          .then(result => {
            this.url = result.data.url;
            Editor.insertEmbed(cursorLocation, "upload", {url: this.url});
            resetUploader();
          })
          .catch(err => {
            console.log(err);
          });
      }
    }
  };
</script>
