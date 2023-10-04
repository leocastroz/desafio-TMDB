<template>
    <select class="p-2 rounded bg-gray-500 text-gray-300 cursor-pointer" :value="timeWindow" @change="onTimeWindowChange">
      <option value="day">Dia</option>
      <option value="week">Semana</option>
    </select>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    props: {
      timeWindow: String,
    },
    emits: ['update:timeWindow', 'fetchMoviesTimeWindow'],
    methods: {
      onTimeWindowChange(event) {
        const selectedValue = event.target.value;
        this.$emit('update:timeWindow', selectedValue);
        this.$emit('fetchMoviesTimeWindow');
      },
    },
  });
  </script>
  