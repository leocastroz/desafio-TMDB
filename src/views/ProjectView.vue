<template>
  <div>
    <div class="w-2/5 mx-auto rounded-xl px-10">
      <div class="flex justify-between items-center bg-gray-800 my-8 px-3 rounded">
        <div>
          <h1 class="py-12 text-white">Lista de Filmes</h1>
        </div>
        <div>
          <select class="p-2 mx-2 rounded bg-gray-500 text-gray-300" v-model="selectedOption" @change="fetchMovies()">
            <option value="now_playing">Now Playing</option>
            <option value="popular">Popular</option>
            <option value="top_rated">Top Rated</option>
            <option value="upcoming">Upcoming</option>
          </select>
          <select class="p-2 rounded bg-gray-500 text-gray-300" v-model="timeWindow" @change="fetchMoviesTimeWindow()">
            <option value="day">Day</option>
            <option value="week">Week</option>
          </select>
        </div>
      </div>
      <ul class="flex flex-wrap gap-4 justify-between">
        <li v-for="movie in movies" :key="movie.id" class="bg-gray-800 w-1/4 rounded-xl text-white">
          <img class="rounded-xl" :src="getMoviePosterUrl(movie.poster_path)" alt="Poster do Filme">
          <p class="my-2 w-full text-center mx-auto">{{ movie.title }}</p>
          <p class="my-2 w-full text-center mx-auto text-gray-500 text-[13px]">{{ formatDate(movie.release_date) }}</p>

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
    const selectedOption = ref('now_playing');
    const timeWindow = ref('day');

    const formatDate = (dateString) => {
      const options = { year: 'numeric', month: 'short', day: 'numeric' };
      const date = new Date(dateString);
      return date.toLocaleDateString('pt-BR', options);
    };

    const fetchMovies = () => {
      // const apiKey = 'SUA_CHAVE_API';
      let apiUrl = '';

      if (selectedOption.value === 'now_playing') {
        apiUrl = `https://api.themoviedb.org/3/movie/now_playing?api_key=c8a98580ce50f04d603d9913e1935107`;
      } else if (selectedOption.value === 'popular') {
        apiUrl = `https://api.themoviedb.org/3/movie/popular?api_key=c8a98580ce50f04d603d9913e1935107`;
      } else if (selectedOption.value === 'top_rated') {
        apiUrl = `https://api.themoviedb.org/3/movie/top_rated?api_key=c8a98580ce50f04d603d9913e1935107`;
      } else if (selectedOption.value === 'upcoming') {
        apiUrl = `https://api.themoviedb.org/3/movie/upcoming?api_key=c8a98580ce50f04d603d9913e1935107`;
      }

      axios
        .get(apiUrl)
        .then(response => {
          movies.value = response.data.results;
        })
        .catch(error => {
          console.error('Erro ao buscar filmes:', error);
        });
    };

    const fetchMoviesTimeWindow = () => {
      let apiUrl = '';

      if (timeWindow.value === 'day') {
        apiUrl = `https://api.themoviedb.org/3/trending/all/day?api_key=c8a98580ce50f04d603d9913e1935107`;
      } else if (timeWindow.value === 'week') {
        apiUrl = `https://api.themoviedb.org/3/trending/all/week?api_key=c8a98580ce50f04d603d9913e1935107`;
      }

      axios
        .get(apiUrl)
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
      selectedOption,
      timeWindow,
      getMoviePosterUrl,
      formatDate,
      fetchMovies,
      fetchMoviesTimeWindow
    };
  }
};
</script>
