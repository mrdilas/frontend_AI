<template>
    <div class="container">
      <div class="image-container">
        <div class="image-area" :style="{ 'background-image': `url(${imageUrl})` }"></div>
        <div class="empty-area"></div>
      </div>
      <div class="button-container">
        <button @click="selectImage">Добавить фотографию</button>
        <button>Обрезать</button>
        <button>Генерировать</button>

      </div>
      <input type="file" ref="fileInput" @change="handleFileInput" style="display: none;" />
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        imageUrl: '',
      };
    },
    methods: {
      selectImage() {
        this.$refs.fileInput.click();
      },
      handleFileInput(event) {
        const file = event.target.files[0];
        this.imageUrl = URL.createObjectURL(file);
      },
    },
  };
  </script>
  
  <style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  
  .image-container {
    display: flex;
    width: 80%;
    height: 400px;
    margin-bottom: 20px;
  }
  
  .image-area,
  .empty-area {
    flex: 1;
    background-color: rgba(255, 255, 255, 0.5);
    margin: 10px;
  }
  
  .image-area {
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
  }
  
  .button-container {
    display: flex;
    justify-content: center;
    gap: 20px;
  }
  
  button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  </style>