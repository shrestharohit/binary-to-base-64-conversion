<template>
  <div>
    <input type="file" @change="handleImage" />
    <span v-if="error">{{ error }}</span>
    <img
      v-if="formValues.profile_picture"
      :src="formValues.profile_picture"
      alt="card_image"
      width="50%"
    />
  </div>
</template>
<script>
  export default {
    data() {
      return {
        error: false,
        fileName: "",
        formValues: {
          profile_picture: null
        }
      };
    },
    methods: {
      // check if Base64 url
      isNewImage(s) {
        return !!s.match(";base64");
      },
      // converts file into base64
      handleImage(e) {
        this.error = false;
        const fileObject = e.target.files[0];
        this.validateImage(fileObject);
        if (!this.error) {
          this.fileName = fileObject.name;
          const reader = new FileReader();
          reader.onload = e => {
            this.formValues.profile_picture = e.target.result;
          };
          reader.readAsDataURL(fileObject);
        }
      },
      // checks if image requirement is met or not
      validateImage(file) {
        if (file.size > 2 * 1024 * 1024) {
          this.error = "File size cannot be greater than 2MB.";
          return;
        }
        const validExtensions = ["jpg", "jpeg", "png"];
        const fileName = file.name.substr(0);
        const extension = fileName.split(".").pop();
        if (!validExtensions.includes(extension)) {
          this.error = `Invalid Extension. Use ${validExtensions.toString()}`;
        }
      }
    }
  };
</script>
