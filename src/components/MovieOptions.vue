<template>
    <div>
      <select class="p-2 mx-2 rounded bg-gray-500 text-gray-300 cursor-pointer" :value="selectedOption" @change="onOptionChange">
        <option value="now_playing">Recentes</option>
        <option value="popular">Populares</option>
        <option value="top_rated">Mais Votados</option>
        <option value="upcoming">Em breve</option>
      </select>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    props: {
      selectedOption: String,
    },
    emits: ['update:selectedOption', 'fetchMovies'],
    methods: {
      onOptionChange(event) {
        const selectedValue = event.target.value;
        this.$emit('update:selectedOption', selectedValue); // Emita o evento personalizado para atualizar a prop
        this.$emit('fetchMovies'); // Emita o evento personalizado para buscar filmes
      },
    },
  });
  </script>
  