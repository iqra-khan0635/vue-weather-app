<template>
  <div class="flex items-center justify-between w-full">
    <div class="w-1/3 text-justify">{{ getDayOfWeek }}</div>
    <div class="w-1/3">
      <img
        :src="day.day.condition.icon"
        alt="weather-icon"
        width="30"
        class="mx-auto"
      />
    </div>
    <div class="w-1/3 text-right" v-html="getTemperature"></div>
  </div>
</template>
<script>
export default {
  props: {
    day: {
      type: Object,
      default: () => {},
    },
  },
  computed: {
    getDayOfWeek() {
      const objDate = new Date(this.day.date);
      return objDate.toLocaleDateString("en-US", { weekday: "long" });
    },
    getTemperature() {
      return `${Math.round(this.day.day.maxtemp_c)}&deg; / ${Math.round(
        this.day.day.mintemp_c
      )}&deg;`;
    },
  },
};
</script>
