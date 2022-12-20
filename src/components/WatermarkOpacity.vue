<template>
  <div class="w-full border-1 container relative">
    <div
      class="flex flex-wrap justify-content-between align-items-center flex-1 gap-2 px-3 shadow-1"
    >
      <h3>Select Watermark Opacity</h3>
      <h3>{{ Opacity }}</h3>
    </div>
    <div class="p-3">
      <Slider v-model="Opacity" @change="OpacityChangeHandler" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Slider from "primevue/slider";

// define events
const emits = defineEmits<{
  (e: "onOpacityChange", Opacity: number): void;
}>();

// define states
const Opacity = ref(50);

// methods
const OpacityChangeHandler = (value) => {
  emits("onOpacityChange", Opacity.value);
};
</script>

<style scoped></style>
