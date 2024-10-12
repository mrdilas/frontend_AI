<template>
  <div class="wrapper-container">
    <div class="wrapper">
      <img :src="imageUrl" alt="Uploaded Image" v-if="imageUrl" />
    </div>
    <div class="wrapper_gen"></div>
  </div>
  <button @click="openFileDialog" class="button">Выбрать фото</button>
  <button class="button">Редактировать фото</button>
  <button class="button_gen">Генерация объекта</button>
  <input type="file" ref="fileInput" @change="handleFileChange" style="display: none" />
</template>

<script>
export default {
  data() {
    return {
      imageUrl: null,
    };
  },
  methods: {
    openFileDialog() {
      this.$refs.fileInput.click();
    },
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.imageUrl = URL.createObjectURL(file);
      }
    },
  },
};
</script>

<style scoped>
.wrapper-container {
  display: flex;
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
</style>
