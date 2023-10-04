<template>
  <div v-if="show" class="fixed inset-0 flex items-center justify-center z-50">
    <div class="absolute inset-0 bg-black opacity-50"></div>
    <div class="relative z-10 bg-white p-8 max-w-md rounded-lg shadow-lg">
      <button @click="closeModal" class="absolute top-4 right-4 text-gray-500 hover:text-gray-700 cursor-pointer">
        <p>x</p>
      </button>
      <div>
        <h2 class="text-xl font-semibold">{{ movie.title }}</h2>
        <p class="pt-2 text-[13px] text-gray-400">{{ movie.overview }}</p>
        <img :src="getBackdropImageUrl(movie.backdrop_path)" alt="Backdrop do Filme" width="200" class="rounded-xl py-3">
        <p class="text-xs py-1 bg-gray-200 w-44 rounded px-4">Pontuação Média - <span class="text-green-500 font-bold">{{movie.vote_average}}</span></p>
        <p class="text-xs mt-2 py-1 bg-gray-200 w-44 rounded px-4">Avaliações - <span class="text-blue-500 font-bold">{{movie.vote_count}}</span></p>
      </div>
    </div>
  </div>
</template>
  
<script>
export default {
  props: {
    show: Boolean,
    movie: Object,
  },
  methods: {
    closeModal() {
      this.$emit("close");
    },
    getBackdropImageUrl(path) {
      if (path) {
        return `https://image.tmdb.org/t/p/w200${path}`;
      }
      return '';
    },
  },
};
</script>