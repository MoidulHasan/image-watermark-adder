<template>
  <div class="w-full border-1">
    <div
      class="flex flex-wrap justify-content-between align-items-center flex-1 gap-2 px-3 shadow-1"
    >
      <Button
        icon="pi pi-cloud-download"
        class="p-button-rounded"
        :disabled="!file"
      />
      <h3>Preview Image</h3>
    </div>

    <div v-if="file" class="mt-2 w-full">
      <img :src="file?.objectURL" alt="" srcset="" class="w-full" />
    </div>

    <div
      v-else
      class="flex align-items-center justify-content-center flex-column mt-5"
    >
      <i class="pi pi-folder border-2 p-5 text-8xl text-400 border-0" />
      <h3 class="my-4">No file selected</h3>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import Button from "primevue/button";

// define props
const props = defineProps<{
  file?: object | null;
}>();

const image = ref(null);

// define events
const emits = defineEmits<{
  (e: "onDownload"): void;
}>();
</script>

<style scoped></style>
