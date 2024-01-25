<template>
  <div
    v-if="weather"
    :class="weather?.current.is_day ? 'bg-day' : 'bg-night'"
    class="rounded-lg text-white p-5 shadow-lg gap-6 my-6 relative overflow-hidden"
  >
    <!-- Location and Time -->
    <div class="flex flex-row justify-between items-center">
      <div class="w-9/12 break-words">
        <i class="fa-solid fa-location-dot"></i> {{ weather.location.name }}
      </div>
      <div class="w-1/4">
        <i class="fa-solid fa-clock"></i> {{ timeFormat }}
      </div>
    </div>

    <!-- Current Weather Info -->
    <div class="flex flex-col text-center my-10">
      <img
        :src="weather?.current?.condition?.icon"
        alt="weather-icon"
        class="mx-auto mb-3 bg-blend-darken"
        style="width: 200px"
      />
      <h1 class="text-8xl">{{ Math.round(weather?.current.temp_c) }}&deg;</h1>
      <p class="text-4xl">{{ weather?.current?.condition?.text }}</p>
    </div>

    <HorizontalLine />
    <div
      class="flex flex-col text-center"
      v-if="weather?.forecast?.forecastday"
    >
      <WeatherForcast
        v-for="(day, idx) in weather.forecast.forecastday"
        :key="idx"
        :day="day"
      />
    </div>
  </div>
</template>
<script>
import HorizontalLine from "./HorizontalLine.vue";
import WeatherForcast from "./WeatherForcast.vue";

export default {
  props: {
    weather: {
      type: Object,
      default: () => {},
    },
  },
  computed: {
    timeFormat() {
      let hours = new Date(this.weather.location.localtime).getHours();
      let minutes = new Date(this.weather.location.localtime).getMinutes();

      const ampm = hours >= 12 ? "PM" : "AM";
      hours = hours % 12 || 12;
      return `${hours}:${minutes < 10 ? "0" : ""}${minutes} ${ampm}`;
    },
  },
  components: {
    HorizontalLine,
    WeatherForcast,
  },
};
</script>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e4d4f4 100%);
}
.bg-night {
  background-color: #3949ab;
  background-image: linear-gradient(62deg, #3949ab 0%, #0a2a4a 100%);
}
</style>
