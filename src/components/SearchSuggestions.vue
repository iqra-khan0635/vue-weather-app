<template>
  <div class="flex flex-col my-2 bg-white border-0 rounded-lg shadow-lg">
    <div
      v-if="loadingSuggestions"
      class="p-2 text-center items-center justify-center"
    >
      <i class="fas fa-spinner fa-pulse text-4xl text-indigo-600"></i>
    </div>

    <div
      v-else-if="suggestedResult && suggestedResult.length"
      class="p-2 text-left hover:text-indigo-600 hover:cursor-pointer hover:font-bold"
      v-for="(data, idx) in suggestedResult"
      :key="idx"
      @click="selectedPlace(data)"
    >
      {{ data.name }}, {{ data.region }}, {{ data.country }}
    </div>
    <div
      v-else-if="suggestedResult !== null"
      class="p-2 text-center items-center justify-center"
    >
      No Result Found
    </div>
  </div>
</template>
<script>
export default {
  props: {
    loadingSuggestions: {
      type: Boolean,
      default: false,
    },
    suggestedResult: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    selectedPlace(place) {
      this.$emit("selected-place", place);
    },
  },
};
</script>
