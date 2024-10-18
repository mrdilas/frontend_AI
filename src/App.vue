<template>
  <link :href="fontDefaultUrl" rel="stylesheet">

  <div class="wrapper-container">
    <div class="wrapper">
      <div class="image-container">
        <img v-if="isItDefaultPhoto" :src="imageDefaultUrl" width="150" height="150" :style="opacityFile">
        <h1 v-if="isItDefaultPhoto" class="pacifico-regular">
          Добавьте <br>
          изображение
        </h1>
        <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" class="uploaded-image"/>
      </div>
    </div>

    <div class="wrapper_gen">
      <div class="image-container">
        <img v-if="generatedImageUrl" :src="generatedImageUrl" alt="Generated Image" />

        <h1 v-if="isItGeneratedPhoto" class="pacifico-regular"> 
          Что AI определил <br>
          и с чем дальше работать будет
        </h1>
      </div>
    </div>
  </div>
  
  <input type="file" ref="fileInput" @change="handleFileChange" style="display:none;" />
  
  <div>
    <button @click="openFileDialog" class="button">
      Выбрать фото
    </button>

    <button @click="deleteFileChange" class="button_delete" :disabled="isItDefaultPhoto">
      Удалить фото
    </button>

    <button @click="editPhoto" class="button" :disabled="isItDefaultPhoto"> 
      Редактировать фото
    </button>
  
    <button @click="uploadFile" class="button_gen" :disabled="isItDefaultPhoto">
      Генерация объекта
    </button>
  </div>

  <div class="wrapper-container">
    <div class="wrapper">
      <h1 class="pacifico-regular"> 
        Здесь будет <br>
        3D модель
      </h1>
    </div>
    <div class="wrapper_gen">
      <h1 class="pacifico-regular">
        Модель, интегрированная <br>
        в окружение
      </h1>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageDefaultUrl: "https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png",
      fontDefaultUrl: "https://fonts.googleapis.com/css2?family=Pacifico&display=swap",
      imageUrl: '',
      generatedImageUrl: '', // Теперь есть отдельный URL для сгенерированного изображения
      isItDefaultPhoto: true,
      isItGeneratedPhoto: true,
      opacityFile: { opacity: 0.2 },
      selectedFile: null,
    };
  },
  methods: {
    openFileDialog() {
      this.$refs.fileInput.click();
    },

    handleFileChange(event) {
      const file = event.target.files[0];
      const supportedFormats = ['image/jpeg', 'image/png', 'image/bmp', 'image/webp'];

      if (file && supportedFormats.includes(file.type)) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageUrl = e.target.result;
          this.isItDefaultPhoto = false;
          this.selectedFile = file; // Сохраняем выбранный файл для отправки на сервер
        };
        reader.readAsDataURL(file);
      } else {
        alert('Пожалуйста, выберите изображение в формате JPEG, PNG, BMP или WEBP.');
      }
    },

    deleteFileChange() {
      this.imageUrl = '';
      this.isItDefaultPhoto = true;
      this.selectedFile = null; // Сбрасываем выбранный файл
      this.$refs.fileInput.value = ''; // Сбрасываем значение input
    },

    async uploadFile() {
      isItGeneratedPhoto = false;
      if (!this.selectedFile) {
        alert('Пожалуйста, выберите файл перед загрузкой.');
        return;
      }

      const formData = new FormData();
      formData.append('file', this.selectedFile);
      
      try {
        const response = await axios.post('http://localhost:8000/segment', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        });

        this.generatedImageUrl = URL.createObjectURL(response.data); // Если сервер возвращает изображение
        this.isItGeneratedPhoto = true; // Установите флаг, если изображение сгенерировано
      } catch (error) {
        console.error('Ошибка при загрузке файла:', error);
      }
    },

    editPhoto() {
      // Логика для редактирования фото (можно добавить позже)
    }
  }
};
</script>

<style scoped>
/* тут нахуй без комментариев, я сам уже не шарю
  что где, ищите по наитию */
.wrapper-container {
  display: flex;
  pointer-events: none;
  justify-content: center;
  margin-top: 20px;
}
.wrapper {
  width: 900px; /* 900px */
  height: 500px; /* 500px*/
  border-radius: 20px;
  background: rgb(240, 230, 213);
  margin: 0 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden; /* Обрезаем все, что выходит за пределы контейнера */
  object-fit: contain;  /* Сохраняем пропорции изображений */
}

.image-container {
  width: 100%; /* Полная ширина контейнер с изображением */
  height: 100%; /* Полная высота контейнера с изображением */
  display: flex; /* Используем Flexbox для центрального выравнивания */
  justify-content: center; /* Центрируем содержимое по горизонтали */
  align-items: center; /* Центрируем содержимое по вертикали */
  overflow: hidden; /* Скрываем все, что выходит за пределы */
} 

.img_file { /* Стили для всех изображений в .wrapper */
  width: 150px; /* Ширина изображения не больше ширины родителя */
  height: 150px; /* Высота изображения не больше высоты родителя */
}
.uploaded-image {
  max-width: 100%;     /* Ширина для загруженных изображений не больше ширины родителя */
  max-height: 100%;    /* Высота для загруженных изображений не больше высоты родителя */
  object-fit: contain;  /* Сохраняем пропорции изображений */
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


.button:disabled {
  background-color: gray;
  cursor: not-allowed;
  opacity: 0.2;
  transform: scale(1) translateY(0px);
}

.button_close_image {
  height: 10px;
  width: 10px;
  border-color: black;
  background-color: white;
  float: right;
  cursor: pointer;
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
  opacity: 0.2;
  transform: scale(1) translateY(0px);
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
  opacity: 0.2;
  transform: scale(1) translateY(0px);
}

.image-container {
  width: 100%; /* Полная ширина контейнер с изображением */
  height: 100%; /* Полная высота контейнера с изображением */
  display: flex; /* Используем Flexbox для центрального выравнивания */
  justify-content: center; /* Центрируем содержимое по горизонтали */
  align-items: center; /* Центрируем содержимое по вертикали */
  overflow: hidden; /* Скрываем все, что выходит за пределы */
}
.img_file {
  opacity: 0.2;
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

