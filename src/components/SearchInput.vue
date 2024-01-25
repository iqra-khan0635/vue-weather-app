<template>
  <div
    class="flex items-center bg-white border border-indigo-600/30 rounded-lg shadow-lg"
  >
    <input
      type="text"
      class="outline-0 border-0 rounded-s-lg p-2 w-full focus:ring-2 focus:ring-indigo-600 ring-inset"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      @keyup.enter="searchPlace"
    />
    <button
      type="button"
      class="bg-indigo-600 rounded-r-lg p-2"
      @click="searchPlace"
    >
      <i class="fa-solid fa-magnifying-glass px-4 text-white"></i>
    </button>
  </div>
</template>
<script>
export default {
  props: ["modelValue"],
  emits: ["update:modelValue", "searchPlaces"],
  methods: {
    searchPlace() {
      this.$emit("searchPlaces", this.modelValue);
    },
  },
};
</script>
