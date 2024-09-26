<template>

  <v-app>
    <v-app-bar :elevation="2">
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <v-app-bar-title>Application Bar</v-app-bar-title>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item link>
            <v-list-item-title>Inicio</v-list-item-title>
        </v-list-item>
        <v-list-item>
          <v-list-item-title>Configuración</v-list-item-title>
        </v-list-item>
        <v-list-item>
          <v-list-item-title prepend-icon="mdi-information-outline">Acerca</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>

  </v-app>

</template>

<script lang="ts" setup>
//
import { ref } from 'vue';

const drawer = ref(false);
</script>