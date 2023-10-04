<template>
  <div>
    <h1 class="w-2/5 mx-auto my-10 p-5 bg-green-800 text-white rounded">Página para desenvolvimento</h1>
    <br>
    <div class="w-2/5 mx-auto bg-red-300 rounded">
      <h1 class="py-12">Lista de Filmes</h1>
      <ul class="flex flex-wrap gap-4 justify-between">
        <li v-for="movie in movies" :key="movie.id">
          <img :src="getMoviePosterUrl(movie.poster_path)" alt="Poster do Filme">
          {{ movie.title }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const movies = ref([]);

    const fetchMovies = () => {
      const apiKey = 'SUA_CHAVE_API';
      axios
        .get('https://api.themoviedb.org/3/discover/movie?api_key=c8a98580ce50f04d603d9913e1935107', {
          params: {
            api_key: apiKey
          }
        })
        .then(response => {
          movies.value = response.data.results;
        })
        .catch(error => {
          console.error('Erro ao buscar filmes:', error);
        });
    };

    const getMoviePosterUrl = posterPath => {
      if (posterPath) {
        return `https://image.tmdb.org/t/p/w200/${posterPath}`;
      }
      return '';
    };

    onMounted(() => {
      fetchMovies();
    });

    return {
      movies,
      getMoviePosterUrl
    };
  }
};
</script>
