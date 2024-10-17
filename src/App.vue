<template>
  
  <link :href = "fontDefaultUrl" rel="stylesheet">

  <div className="wrapper-container">
    <div className="wrapper">
      <div className="image-container">
        <img v-if="isItDefaultPhoto" :src = "imageDefaultUrl" width="150" height="150" :style= "opacityFile">
        <h1 v-if="isItDefaultPhoto" className="pacifico-regular">
          Добавьте <br >
          изображение
        </h1>
        <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" className="uploaded-image"/>
      </div>
    </div>

    <div className="wrapper_gen">
      <h1 className="pacifico-regular"> 
        Что AI опделелил <br >
        и с чем дальше работать будет
      </h1>
    </div>
  </div>
  
  <input type="file" ref="fileInput" @change="handleFileChange" style="display:none;" />
  
  <div>
    <button @click="openFileDialog" className="button">
      Выбрать фото
    </button>

    <button @click="deleteFileChange" className="button_delete" :disabled="isItDefaultPhoto == true">
      Удалить фото
    </button>

    <button @click="" className="button" :disabled="isItDefaultPhoto == true"> 
      Редактировать фото
    </button>
  
    <button className="button_gen" :disabled="isItDefaultPhoto == true">
      Генерация объекта
    </button>
  </div>

  <div className="wrapper-container">
    <div className="wrapper">
      
      <h1 className="pacifico-regular"> 
        Здесь будет <br >
        3D модель</h1>
    </div>
    <div className="wrapper_gen">
      <h1 className="pacifico-regular">
        Модель, интегрированная <br>
        в окружение
      </h1>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      imageDefaultUrl: "https://icons.iconarchive.com/icons/icons8/windows-8/128/Files-Word-icon.png", //ссылка на иконку файла
      fontDefaultUrl: "https://fonts.googleapis.com/css2?family=Pacifico&display=swap", // ссылка на шрифт для текста
      imageUrl: '',
      isItDefaultPhoto: true, // если нет фотографии выводится иконка и текст в первом контейнере 
      opacityFile: { opacity: 0.2 }, //прозрачность для иконки
      imageClose: 'https://icons.iconarchive.com/icons/custom-icon-design/mono-general-1/128/delete-icon.png',
      isItDefaultPhoto: true,
    };
  },
  methods: {
    // функция для выбора картинки
    openFileDialog() {
      this.$refs.fileInput.click();
    },
    
    // функция для добавление в imageUrl выбранной картинки
    handleFileChange(event) {
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

    // функция для удаления картинки из imageUrl
    deleteFileChange() {
      this.imageUrl = '';
      this.isItDefaultPhoto = true;
      this.$refs.fileInput.value = ''; // Сбрасываем значение input, чтобы можно было заново выбрать тот же файл
    },
  },
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

