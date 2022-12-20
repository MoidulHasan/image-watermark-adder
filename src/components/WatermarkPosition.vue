<template>
  <div class="w-full border-1 container relative">
    <div
      class="flex flex-wrap justify-content-between align-items-center flex-1 gap-2 px-3 shadow-1"
    >
      <h3>Select Watermark positon</h3>
    </div>
    <div class="p-3">
      <div
        class="relative h-26rem flex align-items-center justify-content-center"
      >
        <Button
          :class="[
            'absolute bottom-0 right-0 p-3 text-white font-bold border-0 outline-none',
            position === 'lowerRight'
              ? 'p-button-primary'
              : 'p-button-secondary',
          ]"
          @click="onSelectPosition('lowerRight')"
        >
          Bottom Right
        </Button>

        <Button
          :class="[
            'absolute bottom-0 left-0 p-3 text-white font-bold border-0 outline-none',
            position === 'lowerLeft'
              ? 'p-button-primary'
              : 'p-button-secondary',
          ]"
          @click="onSelectPosition('lowerLeft')"
        >
          Bottom Left
        </Button>

        <Button
          :class="[
            'absolute top-0 right-0 p-3 text-white font-bold border-0 outline-none',
            position === 'upperRight'
              ? 'p-button-primary'
              : 'p-button-secondary',
          ]"
          @click="onSelectPosition('upperRight')"
        >
          Top Right
        </Button>

        <Button
          :class="[
            'absolute top-0 left-0 p-3 text-white font-bold border-0 outline-none',
            position === 'upperLeft'
              ? 'p-button-primary'
              : 'p-button-secondary',
          ]"
          @click="onSelectPosition('upperLeft')"
        >
          Top Left
        </Button>

        <Button
          :class="[
            'p-3 text-white font-bold border-0 outline-none',
            position === 'center' ? 'p-button-primary' : 'p-button-secondary',
          ]"
          @click="onSelectPosition('center')"
        >
          Center
        </Button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Button from "primevue/button";

// define events
const emits = defineEmits<{
  (e: "onPositionChange", positon: string): void;
}>();

// define states
const position = ref("lowerLeft");

// methods
const onSelectPosition = (selectedPosition: string) => {
  console.log("Position: ", selectedPosition);
  position.value = selectedPosition;
  emits("onPositionChange", selectedPosition);
};
</script>

<style scoped>
.container {
  height: 500px;
}
</style>
