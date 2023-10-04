<template>
  <div>
    <div class="mx-auto rounded-xl px-10">
      <div class="grid items-center justify-center items-center bg-gray-800 my-8 px-3 rounded sm:flex sm:justify-between">
        <div>
          <h1 class="py-6 text-white text-center">Lista de Filmes</h1>
        </div>
        <div class="py-4 flex">
          <MovieOptions :selectedOption="selectedOption" @update:selectedOption="selectedOption = $event" @fetchMovies="fetchMovies" />
          <DayWeek :DayWeek="DayWeek" @update:DayWeek="DayWeek = $event" @fetchMoviesDayWeek="fetchMoviesDayWeek" />
        </div>
      </div>
      <ul class="flex flex-wrap gap-5 justify-center">
        <li v-for="movie in movies" :key="movie.id" class="bg-gray-800 rounded-xl text-white cursor-pointer hover:scale-105 transition-transform duration-300 hover:bg-blue-700" @click="openModal(movie)">
          <div class="w-36">
            <img class="rounded-xl" :src="getMoviePosterUrl(movie.poster_path)" alt="Poster do Filme" width="150">
            <p class="my-2 w-full text-center mx-auto">{{ movie.title }}</p>
            <p class="my-2 w-full text-center mx-auto text-gray-500 text-[13px]">{{ formatDate(movie.release_date) }}</p>
          </div>
        </li>
      </ul>
    </div>
    <movie-modal :show="showModal" :movie="selectedMovie" @close="closeModal" />
  </div>
</template>

<script>
import MovieOptions from '@/components/MovieOptions.vue';
import MovieModal from '@/components/MovieModal.vue';
import DayWeek from '@/components/DayWeek.vue';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const API_KEY = 'c8a98580ce50f04d603d9913e1935107';

const MOVIE_API_URLS = {
  now_playing: 'https://api.themoviedb.org/3/movie/now_playing',
  popular: 'https://api.themoviedb.org/3/movie/popular',
  top_rated: 'https://api.themoviedb.org/3/movie/top_rated',
  upcoming: 'https://api.themoviedb.org/3/movie/upcoming',
};

const TRENDING_API_URLS = {
  day: 'https://api.themoviedb.org/3/trending/all/day',
  week: 'https://api.themoviedb.org/3/trending/all/week',
};

export default {
  components: {
    MovieOptions,
    MovieModal,
    DayWeek,
  },
  setup() {
    const movies = ref([]);
    const selectedOption = ref('now_playing');
    const DayWeek = ref('day');
    const showModal = ref(false);
    const selectedMovie = ref([]);

    const formatDate = (dateString) => {
      const options = { year: 'numeric', month: 'short', day: 'numeric' };
      const date = new Date(dateString);
      return date.toLocaleDateString('pt-BR', options);
    };

    const fetchMovies = () => {
      const apiUrl = MOVIE_API_URLS[selectedOption.value];
      axios
        .get(apiUrl, { params: { api_key: API_KEY } })
        .then((response) => {
          movies.value = response.data.results;
        })
        .catch((error) => {
          console.error('Erro ao buscar filmes:', error);
        });
    };

    const fetchMoviesDayWeek = () => {
      const apiUrl = TRENDING_API_URLS[DayWeek.value];
      axios
        .get(apiUrl, { params: { api_key: API_KEY } })
        .then((response) => {
          movies.value = response.data.results;
        })
        .catch((error) => {
          console.error('Erro ao buscar filmes:', error);
        });
    };

    const getMoviePosterUrl = (posterPath) => {
      if (posterPath) {
        return `https://image.tmdb.org/t/p/w200/${posterPath}`;
      }
      return '';
    };

    const openModal = (movie) => {
      selectedMovie.value = movie;
      showModal.value = true;
    };

    const closeModal = () => {
      showModal.value = false;
    };

    onMounted(() => {
      fetchMovies();
    });

    return {
      movies,
      showModal,
      openModal,
      formatDate,
      closeModal,
      DayWeek,
      fetchMovies,
      selectedMovie,
      selectedOption,
      getMoviePosterUrl,
      fetchMoviesDayWeek,
    };
  },
};
</script>