<template>

  <link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">

  <div class="wrapper-container">
    <div class="wrapper">
      <div>
        <img v-if="photo" className="img_file" src="https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png" width="150" height="150">
        <h1 v-if="photo" className="pacifico-regular">Вставьте <br >
                                                      изображение</h1>
        <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" />
      </div>
    </div>
    <div class="wrapper_gen">
      <h1 className="pacifico-regular"> Что AI опделелил <br >
                                        и с чем дальше работать будет
      </h1>
    </div>
  </div>

  <button @click="openFileDialog" class="button">Выбрать фото</button>
  <button class="button">Редактировать фото</button>
  <button className="button_gen">Генерация объекта</button>
  <input type="file" ref="fileInput" @change="handleFileChange" style="display: none" />

  <div class="wrapper-container">
    <div class="wrapper">
      
      <h1 className="pacifico-regular"> Здесь будет <br >
                                        3D модель</h1>
    </div>
    <div class="wrapper_gen">
      <h1 className="pacifico-regular">Здась тоже что то типо того</h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageUrl: '',
      photo: true,
    };
  },
  methods: {
    openFileDialog() {
      this.$refs.fileInput.click();
    },

    //___________________функция для проверки выбранного файла____________________
    handleFileChange(event) {
      const file = event.target.files[0];
      // Поддерживаемые форматы изображений
      const supportedFormats = ['image/jpeg', 'image/png', 'image/gif', 'image/bmp', 'image/webp'];
      // Проверка типа файла
      if (file && supportedFormats.includes(file.type)) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageUrl = e.target.result;
          this.photo = false;
        };
        reader.readAsDataURL(file);
      } else {
        alert('Пожалуйста, выберите изображение в формате JPEG, PNG, GIF, BMP или WEBP.');
      }
    },
  },
};
</script>

<style scoped>
.wrapper-container {
  display: flex;
  pointer-events: none;
  justify-content: center;
  margin-top: 20px;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 20px;
  background: rgb(240, 230, 213);
  margin: 0 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
.wrapper img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}
.wrapper_gen {
  width: 900px;
  height: 500px;
  border-radius: 20px;
  background: rgb(240, 230, 213);
  margin: 0 20px;
  justify-content: center;
  align-items: center;
  display: flex;
}
button {
  background: rgb(241, 173, 46);
  color: white;
  border-radius: 10px;
  border: 2px solid rgb(245, 245, 115);
  margin-left: 20px;
  padding: 10px 15px;
  cursor: pointer;
  transition: transform 500ms ease;
  margin-top: 20px;
  font-size: 16px;
}
button:hover {
  transform: scale(1.1) translateY(-5px);
}
.button_gen {
  background: rgb(30, 128, 17);
  color: white;
  border-radius: 10px;
  border: 2px solid rgb(18, 77, 10);
  margin-left: 20px;
  padding: 10px 15px;
  cursor: pointer;
  transition: transform 500ms ease;
  margin-top: 20px;
  font-size: 16px;
}
.button_gen:hover {
  transform: scale(1.1) translateY(-5px);
}

.img_file {
  opacity: 0.2;
  display: block;
  margin: 0 auto; /* Убирает отступы */
}

.pacifico-regular {
  font-family: "Pacifico", cursive;
  font-weight: 400;
  font-style: normal;
  text-align: center;
  font-size: 45px;
  opacity: 0.3;
}
</style>