<!-- Vista peliculas, desde esta página podremos ver la tabla peliculas y las acciones solicitadas -->
<template>
  <div>
    <v-container>
      <!-- Código para cuadro de error -->
      <v-alert v-if="connectionError" type="error" dense colored-border class="mb-3" dismissible
        @input="connectionError = false">
        <v-icon left>mdi-alert-circle-outline</v-icon>
        Error de conexión
      </v-alert>

      <!-- Tabla peliculas -->
      <h2 v-if="selectedMovie" class="selected-movie">Película: {{ selectedMovie }}</h2>
      <v-data-table :headers="headers" :items="movies" item-value="id" class="elevation-1">
        <template #top>
          <caption>Películas</caption>
        </template>

        <template #item.movie="{ item }">
          <span @click="selectMovie(item.movie)" style="cursor: pointer;">
            {{ item.movie }}
          </span>
        </template>

        <template #item.imdb_url="{ item }">
          <a :href="item.imdb_url" target="_blank">Ver en IMDb</a>
        </template>

        <!-- Iconos paraa edición y borrar -->
        <template #item.actions="{ item }">
          <v-icon small @click.stop="openEditDialog(item)">mdi-pencil-outline</v-icon>
          <v-icon small @click.stop="openDeleteDialog(item)">mdi-delete-outline</v-icon>
        </template>
      </v-data-table>
    </v-container>

    <!-- Cuadro de diálogo para editar -->
    <v-dialog v-model="isEditDialogOpen" max-width="500px">
      <v-card>
        <v-card-title>Editar Película</v-card-title>
        <v-card-text>
          <v-form ref="editForm">
            <v-text-field label="Id" v-model="editableMovie.id" readonly></v-text-field>
            <v-text-field label="Movie" v-model="editableMovie.movie"></v-text-field>
            <v-text-field label="Rating" v-model="editableMovie.rating"></v-text-field>
            <v-text-field label="IMDb URL" v-model="editableMovie.imdb_url"></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="submitEdit">Submit</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Cuadro de diálogo para borrar pelicula -->
    <v-dialog v-model="isDeleteDialogOpen" max-width="400px">
      <v-card>
        <v-card-title class="headline">Borrar?</v-card-title>
        <v-card-text>
          Borrar película: {{ movieToDelete?.movie }}
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="isDeleteDialogOpen = false">Cancelar</v-btn>
          <v-btn color="red" text @click="deleteMovieConfirmed">Borrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

interface Movie {
  id: number;
  movie: string;
  rating: string;
  imdb_url: string;
}

export default defineComponent({
  emits: ['movie-selected'],
  setup(_, { emit }) {
    const movies = ref<Movie[]>([]);
    const selectedMovie = ref<string>('');
    const isEditDialogOpen = ref(false);
    const isDeleteDialogOpen = ref(false);
    const editableMovie = ref<Movie>({
      id: 0,
      movie: '',
      rating: '',
      imdb_url: '',
    });
    const movieToDelete = ref<Movie | null>(null);
    const connectionError = ref(false); // Estado para el error de conexión

    const headers = ref([ //ENcabezados para tabla peliculas
      { text: 'Título', value: 'movie' },
      { text: 'Calificación', value: 'rating' },
      { text: 'IMDb', value: 'imdb_url' },
      { text: 'Acciones', value: 'actions', sortable: false },
    ]);

    const fetchMovies = async () => { //Solicitud de peliculas
      try {
        const response = await fetch('https://dummyapi.online/api/movies');
        if (!response.ok) throw new Error('Error de conexión');
        const data: Movie[] = await response.json();
        movies.value = data;
      } catch (error) {
        console.error('Error al obtener las películas:', error);
        connectionError.value = true; // Muestra el cuadro de error
      }
    };

    const selectMovie = (title: string) => { // Selección de pelicula al dar click en titulo
      selectedMovie.value = title;
      emit('movie-selected', title);
    };

    const openEditDialog = (movie: Movie) => { // Acción para el cuadro de diálogo para editar
      editableMovie.value = { ...movie };
      isEditDialogOpen.value = true;
    };

    const submitEdit = () => { //REalizar cambio en los datos de la tabla peliculas de acuerdo al atributo dado.
      const index = movies.value.findIndex(m => m.id === editableMovie.value.id);
      if (index !== -1) {
        movies.value[index] = { ...editableMovie.value };
      }
      isEditDialogOpen.value = false;
    };

    const openDeleteDialog = (movie: Movie) => { //Acción para el cuadro de diálogo para borrar
      movieToDelete.value = movie;
      isDeleteDialogOpen.value = true;
    };

    const deleteMovieConfirmed = () => { //Realizar el borrado de la pelicula seleccionada.
      if (movieToDelete.value) {
        movies.value = movies.value.filter(m => m.id !== movieToDelete.value.id);
      }
      isDeleteDialogOpen.value = false;
    };

    onMounted(() => {
      fetchMovies();
    });

    return {
      movies,
      headers,
      selectedMovie,
      isEditDialogOpen,
      isDeleteDialogOpen,
      editableMovie,
      movieToDelete,
      connectionError,
      openEditDialog,
      submitEdit,
      openDeleteDialog,
      deleteMovieConfirmed,
      selectMovie,
    };
  },
});
</script>

<style scoped>
/* Estilo para tabla */
.selected-movie {
  text-align: center;
  margin-bottom: 15px;
  font-size: 20px;
}
</style>