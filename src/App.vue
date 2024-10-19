<template>
  <div class="row">
    <div class="workArea">
      <button v-if="!isItDefaultImage" :style="{ color: buttonColor }"
              class="closeButton" @click="deleteImage">&#10006;</button>
      
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

    <div class="workArea">

    </div>

  </div>
  <div class="row">
    <button class="startButton" @click="sendImageToAPI" :disabled="!uploadedImageURL">
    Start
    </button>
  </div>

  <div class="row">

    <div class="workArea">

    </div>  

    <div class="workArea">

    </div>

  </div>
</template>


<script>
export default {
  data() {
    return {
      defaultImageURL: "https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png",
      uploadedImageURL: "",
      isItDefaultImage: true,
      buttonColor: 'black' // херь, чтоб кнопка меняла цвет
    }
  },
  methods: {
    triggerFileInput() {
      this.$refs.fileInput.click();
    },
    uploadImage(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        this.uploadedImageURL = e.target.result;
        this.isItDefaultImage = false;
        this.determineButtonColor(e.target.result); // изменение цвета кнопки
      };
      if (file) {
        reader.readAsDataURL(file);
      }
    },
    determineButtonColor(imageData) {
      const img = new Image();
      img.src = imageData;
      img.onload = () => {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = img.width;
        canvas.height = img.height;
        ctx.drawImage(img, 0, 0);
        
        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const data = imageData.data;

        // вычисление средней яркости
        let r = 0, g = 0, b = 0;
        const pixelCount = data.length / 4;
        for (let i = 0; i < data.length; i += 4) {
          r += data[i];
          g += data[i + 1];
          b += data[i + 2];
        }

        r /= pixelCount;
        g /= pixelCount;
        b /= pixelCount;
        
        // формула яркости
        const brightness = (0.299 * r + 0.587 * g + 0.114 * b) / 255;

        // изменение цвета кнопки через тернарник
        this.buttonColor = brightness < 0.5 ? 'white' : 'black';
      };
    },
    deleteImage() {
      this.uploadedImageURL = "";
      this.isItDefaultImage = true;
      this.buttonColor = 'black'; // сброс цвета кнопки
      this.$refs.fileInput.value = null; 
    },
    sendImageToAPI() {
      // Извлекаем base64 строку изображения
      const base64Image = this.uploadedImageURL.split(',')[1];

      // Настройка API URL
      const apiURL = "https://example.com/upload"; // Замените на ваш URL API

      fetch(apiURL, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ image: base64Image }), // Передать изображение в формат json
      })
      .then(response => response.json())
      .then(data => {
        console.log('Success:', data);
        alert('Image successfully uploaded!');
      })
      .catch((error) => {
        console.error('Error:', error);
        alert('Error uploading image.');
      });
    }
  }
}
</script>

<style scoped>

.row{
  flex-direction: row;
  display: flex;
  margin: 20px;
  align-items: center;
  justify-content: center;
}

.workArea {
  background-color: rgb(241, 241, 241);
  border: 4px solid rgb(179, 179, 179);
  height: 400px;
  width: 600px;
  margin-left: 10px;;
  margin-right: 10px;
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
  height: 100px;
  width: 100px;
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
  background-color: rgba(255, 255, 255, 0);;
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
  object-fit: contain;
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
.startButton {
  height: 50px;
  width: 100px;
  border: none;
  font-size: 20px;
  border-radius: 16px;
  background-color: #09af11;
  color: white;
  cursor: pointer;
  align-items: center;
  display: flex;
  justify-content: center;
}
.startButton:hover {
  background-color: #109116;
}
.startButton:disabled{
  background-color: gray;
  cursor: default;
}
</style>
