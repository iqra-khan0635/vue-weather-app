<template>
  <div class="min-h-screen bg-zinc-100 p-8">
    <main>
      <!-- Search Input -->
      <div class="relative">
        <SearchInput v-model="search" @searchPlaces="searchPlaces" />

        <!-- Search suggestions -->

        <div class="absolute z-10 w-full">
          <SearchSuggestions
            :loadingSuggestions="loadingSuggestions"
            :suggestedResult="suggestedResult"
            @selectedPlace="selectedPlace"
          />
        </div>
      </div>

      <!-- Weather Card -->
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <WeatherCard
          :weather="data"
          v-for="(data, idx) in weatherList"
          :key="idx"
        />
      </div>
    </main>
  </div>
</template>

<script>
import SearchInput from "./components/SearchInput.vue";
import SearchSuggestions from "./components/SearchSuggestions.vue";
import WeatherCard from "./components/WeatherCard.vue";
import axios from "axios";

export default {
  components: {
    SearchInput,
    SearchSuggestions,
    WeatherCard,
  },
  data() {
    return {
      API_KEY: "1782fa6b14424802b20162502241801",
      search: "",
      loadingSuggestions: false,
      suggestedResult: null,
      weatherList: [],
    };
  },
  watch: {
    search() {
      this.suggestedResult = null;
    },
  },
  methods: {
    async searchPlaces() {
      if (this.search && this.search.length >= 3) {
        this.loadingSuggestions = true;
        const params = {
          key: this.API_KEY,
          q: this.search,
        };

        await axios
          .get(`http://api.weatherapi.com/v1/search.json`, { params })
          .then((response) => {
            this.suggestedResult = response.data;
          })
          .catch((error) => {
            console.error("Error:", error);
          })
          .finally(() => {
            this.loadingSuggestions = false;
          });
      }
    },
    async selectedPlace(place) {
      this.search = ""; //`${place.name}, ${place.region}, ${place.country}`;
      this.getWeather(place);
      this.suggestedResult = null;
    },
    async getWeather(place) {
      const params = {
        key: this.API_KEY,
        days: 3,
        aqi: "no",
        alerts: "no",
        q: `id:${place.id}`,
      };

      await axios
        .get(`http://api.weatherapi.com/v1/forecast.json`, { params })
        .then((response) => {
          this.weatherList.push(response.data);
        })
        .catch((error) => {
          console.error("Error:", error);
        })
        .finally(() => {});
    },
  },
};
</script>
