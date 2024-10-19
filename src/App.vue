<template>
  <div class="workArea">
    <button v-if="!isItDefaultImage" class="closeButton" 
            @click="deleteImage">&#10006;</button>
    
    <img v-if="isItDefaultImage" class="defaultImage" :src="defaultImageURL" alt="Default Image">
    <h1 v-if="isItDefaultImage">
      Import image
    </h1>
    <img v-else class="uploadedImage" :src="uploadedImageURL">

    <button class="uploadButton" @click="triggerFileInput">
      Upload Image
    </button>

    <input type="file" ref="fileInput" @change="uploadImage" accept="image/*" class="uploadInput" style="display: none;" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      defaultImageURL: "https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png",
      uploadedImageURL: "",
      isItDefaultImage: true
    }
  },
  methods: {
    triggerFileInput() {
      // Программно вызываем файловый input
      this.$refs.fileInput.click();
    },
    uploadImage(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        this.uploadedImageURL = e.target.result;
        this.isItDefaultImage = false;
      };
      if (file) {
        reader.readAsDataURL(file);
      }
    },
    deleteImage() {
      this.uploadedImageURL = "";
      this.isItDefaultImage = true;
    }
  }
}
</script>

<style scoped>
.workArea {
  background-color: rgb(241, 241, 241);
  border: 4px solid rgb(179, 179, 179);
  height: 400px;
  width: 600px;
  border-radius: 40px;
  overflow: hidden;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.workArea h1 {
  color: black;
  opacity: 0.3;
  text-align: center;
  z-index: 2;
}

.defaultImage {
  height: 75px;
  width: 75px;
  opacity: 0.3;
  -webkit-user-drag: none;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 45px;
  height: 45px;
  border: none;
  border-radius: 10px;
  color: black;
  background-color: white;
  font-size: 40px;
  cursor: pointer;
  z-index: 3;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 0;
}

.closeButton:focus {
  outline: none;
}

.uploadedImage {
  height: 100%;
  width: 100%;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  -webkit-user-drag: none;
}

.uploadButton {
  margin-top: 20px;
  padding: 10px 20px;
  border: none;
  border-radius: 16px;
  background-color: #007BFF;
  color: white;
  cursor: pointer;
}

.uploadButton:hover {
  background-color: #3580cf;
}
</style>
