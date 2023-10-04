<template>
  <div>
    <select
      class="p-2 mx-2 rounded bg-gray-500 text-gray-300 cursor-pointer text-[13px]"
      :value="selectedOption"
      @change="OptionChange"
    >
      <option value="now_playing">Recentes</option>
      <option value="popular">Populares</option>
      <option value="top_rated">Mais Votados</option>
      <option value="upcoming">Em breve</option>
    </select>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    selectedOption: String,
  },
  emits: ["update:selectedOption", "fetchMovies"],
  methods: {
    OptionChange(event) {
      const selectedValue = event.target.value;
      this.$emit("update:selectedOption", selectedValue);
      this.$emit("fetchMovies");
    },
  },
});
</script>
