<template>
  
  <link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">

  <div className="wrapper-container">
    <div className="wrapper">
      <div>
        <img v-if="isItDefaultPhoto == true" className="img_file" :src = "imageDefaultUrl" width="150" height="150">
        <h1 v-if="isItDefaultPhoto == true" className="pacifico-regular">
          Вставьте <br >
          изображение
        </h1>
        <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" />
      </div>
    </div>
    <div class="wrapper_gen">
      <h1 className="pacifico-regular"> 
        Что AI опделелил <br >
        и с чем дальше работать будет
      </h1>
    </div>
  </div>

  <div className="wrapper_button">
    <button @click="openFileDialog" class="button">
      Выбрать фото
    </button>

    <button @click="deleteFileChange" className="button_delete" :disabled="isItDefaultPhoto == true">
      Удалить фото
    </button>

    <button class="button">
      Редактировать фото
    </button>
  
    <button className="button_gen" :disabled="isItDefaultPhoto == true">
      Генерация объекта
    </button>
  </div>

  <input type="file" ref="fileInput" @change="handleFileChange" style="display: none" />

  <div class="wrapper-container">
    <div class="wrapper">
      
      <h1 className="pacifico-regular"> 
        Здесь будет <br >
        3D модель</h1>
    </div>
    <div class="wrapper_gen">
      <h1 className="pacifico-regular">
        Здась тоже что то типо того
      </h1>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      imageDefaultUrl: "https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png",
      imageUrl: '',
      isItDefaultPhoto: true,
    };
  },
  methods: {
    openFileDialog() {
      this.$refs.fileInput.click();
    },
    
    //___________________функция для добавление в imageUrl выбранного файла____________________
    handleFileChange() {
      const file = event.target.files[0];
      // Поддерживаемые форматы изображений
      const supportedFormats = ['image/jpeg', 'image/png', 'image/bmp', 'image/webp'];
      // Проверка типа файла
      if (file && supportedFormats.includes(file.type)) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageUrl = e.target.result;
          this.isItDefaultPhoto = false;
        };
        reader.readAsDataURL(file);
      } else {
        alert('Пожалуйста, выберите изображение в формате JPEG, PNG, BMP или WEBP.');
      }
    },
    //___________________функция для удаления картинки из imageUrl____________________
    deleteFileChange() {
      this.imageUrl = '';
      this.isItDefaultPhoto = true;
      this.$refs.fileInput.value = ''; // Сбрасываем значение input, чтобы можно было заново выбрать тот же файл
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
  padding: 10px 15px;
  cursor: pointer;
  transition: transform 500ms ease;
  font-size: 16px;
}
.button_gen:hover {
  transform: scale(1.1) translateY(-5px);
}
.button_gen:disabled {
  background-color: gray;
  cursor: not-allowed;
}

.button_delete {
  background: rgb(212, 30, 30);
  color: white;
  border-radius: 10px;
  border: 2px solid rgb(110, 4, 4);
  margin-left: 20px;
  padding: 10px 15px;
  cursor: pointer;
  transition: transform 500ms ease;
  margin-top: 20px;
  font-size: 16px;
}
.button_delete:hover {
  transform: scale(1.1) translateY(-5px);
}
.button_delete:disabled {
  background-color: gray;
  cursor: not-allowed;
}

.wrapper-button {
  display: flex;
  justify-content: flex-start; /* Выравнивание остальных кнопок по левому краю */
  align-items: center; /* Центрируем кнопки по вертикали */
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

