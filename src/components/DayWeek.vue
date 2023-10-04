<template>
  <select
    class="p-2 rounded bg-gray-500 text-gray-300 cursor-pointer text-[13px]"
    :value="DayWeek"
    @change="ChangeDayWeek"
  >
    <option value="day">Dia</option>
    <option value="week">Semana</option>
  </select>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    DayWeek: String,
  },
  emits: ["update:DayWeek", "fetchMoviesDayWeek"],
  methods: {
    ChangeDayWeek(event) {
      const selectedValue = event.target.value;
      this.$emit("update:DayWeek", selectedValue);
      this.$emit("fetchMoviesDayWeek");
    },
  },
});
</script>
