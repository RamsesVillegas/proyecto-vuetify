<!-- App principal de vuestify, en ella definimos el menu despegable y las opciones -->
<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>Mi SuperApp</v-list-item-title>
            <v-list-item-subtitle>By Me</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <!-- Elemento Inicio -->
        <RouterLink to="/inicio" custom v-slot="{ navigate }">
        <v-list-item @click="navigate" link>
            <v-icon>mdi-border-all</v-icon>
            <v-list-item-title>Inicio</v-list-item-title>
        </v-list-item>
        </RouterLink>

        <!-- Elemento Configuración -->
        <RouterLink to="/peliculas" custom v-slot="{ navigate }">
          <v-list-item @click="navigate" link>
            <v-icon>mdi-movie-open-outline</v-icon>
            <v-list-item-title>Peliculas</v-list-item-title>
        </v-list-item>
        </RouterLink>

        <!-- Elemento Acerca -->
        <v-list-item link>
            <v-icon>mdi-information-outline</v-icon>
            <v-list-item-title>Acerca</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app :elevation="2">
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <v-app-bar-title>SuperApp</v-app-bar-title>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const drawer = ref(false);
</script>
