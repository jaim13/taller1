<template>
  <div class="movies">
    <h1>Catálogo de Películas</h1>

    <div class="movie-grid">
      <div
        class="movie-card"
        v-for="movie in movies"
        :key="movie.id"
        @click="selectMovie(movie)"
      >
        <img
          :src="getImageUrl(movie.poster_path)"
          :alt="movie.title"
          class="movie-image"
        />

        <div class="movie-details">
          <h2>{{ movie.title }}</h2>

          <p>Fecha de lanzamiento: {{ movie.release_date }}</p>

          <p class="movie-overview">{{ getShortOverview(movie.overview) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MovieCatalog",

  data() {
    return {
      movies: [],
    };
  },

  created() {
    this.getMovies();
  },

  methods: {
    async getMovies() {
      try {
        const apiKey = "69b5896d2043aae821844383ab78a9ff"; // Reemplaza con tu clave de API de TMDb

        const response = await axios.get(
          "https://api.themoviedb.org/3/discover/movie",
          {
            params: {
              api_key: apiKey,

              // Puedes agregar más parámetros de consulta aquí según tus necesidades, como género, año de lanzamiento, etc.
            },
          }
        );

        this.movies = response.data.results.map((movie) => ({
          ...movie,

          // Construye la URL completa de la imagen de la película

          poster_path: movie.poster_path
            ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
            : null,
        }));
      } catch (error) {
        console.error(error);
      }
    },

    getImageUrl(posterPath) {
      return posterPath || "placeholder-image-url"; // Agrega una URL de imagen de relleno si la película no tiene una imagen disponible
    },

    getShortOverview(overview) {
      const maxLength = 100; // Ajusta este valor según la longitud deseada

      if (overview.length > maxLength) {
        return overview.substring(0, maxLength) + "...";
      }

      return overview;
    },

    selectMovie(movie) {
      // Aquí puedes manejar lo que sucede cuando se selecciona una película, como abrir un modal con más detalles, etc.

      console.log("Seleccionaste la película:", movie.title);
    },
  },
};
</script>

<style scoped>
.movies {
  margin-top: 20px;
}

.movie-grid {
  display: grid;

  grid-template-columns: repeat(
    auto-fill,
    minmax(300px, 1fr)
  ); /* Cambia el ancho mínimo y máximo */

  gap: 30px; /* Aumenta el espacio entre las tarjetas */
}

.movie-card {
  cursor: pointer;

  border: 1px solid #ccc;

  border-radius: 8px;

  overflow: hidden;

  transition: transform 0.3s ease;

  width: 300px; /* Ancho de la tarjeta */

  height: 480px; /* Aumenta la altura de la tarjeta para acomodar la descripción */
}

.movie-card:hover {
  transform: scale(1.05);
}

.movie-image {
  width: 100%;

  height: 60%; /* Ajusta la altura de la imagen */

  object-fit: cover; /* Para que la imagen cubra el contenedor sin deformarse */
}

.movie-details {
  padding: 10px;
}

.movie-details h2 {
  margin-bottom: 5px;
}

.movie-overview {
  margin-top: 10px;

  font-size: 0.9em;

  color: #666;
}
</style>
