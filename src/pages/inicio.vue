<!-- Vista inicio, la cual tiene como funcion mostrar una imagen y cambiarla por otra al momento de presionar un botón-->
<template>
  <div class="home-container">
    <!-- Indicador de carga para la imagen -->
    <v-progress-linear
      v-if="isLoading"
      indeterminate
      color="primary"
      class="mb-3"
    ></v-progress-linear>

    <!-- Se agrega el botón y el cuadro para la imagen. -->
    <img :src="imageUrl" class="centered-image" @load="isLoading = false" @error="handleError" />
    <button @click="refreshImage" class="change-image-button">Siguiente</button>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

// Estado de la URL de la imagen y de la conexión
const baseUrl = 'https://picsum.photos/500';
const imageUrl = ref(`${baseUrl}?refresh=${Date.now()}`);
const isLoading = ref(false); // Estado para controlar el indicador de carga

// Función para refrescar la imagen
const refreshImage = () => {
  isLoading.value = true;
  imageUrl.value = `${baseUrl}?refresh=${Date.now()}`;
};

</script>

<style scoped>
/* estilos ccs para el botón, imagen y cuadros. */
.home-container {
  text-align: center; 
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}

.centered-image {
  max-width: 100%;
  height: auto;
}

.change-image-button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #6200ea;
  color: white;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.change-image-button:hover {
  background-color: #3700b3;
}
</style>
