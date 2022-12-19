<template>
  <div>
    <FileUpload
      name="images[]"
      :multiple="multiple"
      accept="image/*"
      @select="onSelectedFiles"
    >
      <template #header="{ chooseCallback, clearCallback, files }">
        <div
          class="flex flex-wrap justify-content-between align-items-center flex-1 gap-2"
        >
          <div class="flex gap-2">
            <Button
              @click="chooseCallback()"
              icon="pi pi-images"
              class="p-button-rounded"
            />
            <Button
              @click="clearCallback()"
              icon="pi pi-times"
              class="p-button-rounded p-button-danger"
              :disabled="!files || files.length === 0"
            />
          </div>
          <h3>{{ title }}</h3>
        </div>
      </template>

      <template #content="{ files }">
        <div v-if="files.length > 0">
          <!-- <div
            v-for="file of files"
            :key="file.name + file.type + file.size"
            class="m-0 w-full"
          >
            <div>
              <img
                :alt="file.name"
                :src="file.objectURL"
                class="w-full"
                height="200"
              />
            </div>
          </div> -->
          <Galleria
            :value="files"
            :responsiveOptions="responsiveOptions"
            :numVisible="5"
            containerStyle="max-width: 640px"
          >
            <template #item="slotProps">
              <img :src="slotProps.item.originalUrl" style="width: 100%" />
            </template>
          </Galleria>
        </div>
      </template>

      <template #empty>
        <div class="flex align-items-center justify-content-center flex-column">
          <i
            class="pi pi-cloud-upload border-2 border-circle p-5 text-8xl text-400 border-400"
          />
          <p class="mt-4 mb-0">Drag and drop files to here to upload.</p>
        </div>
      </template>
    </FileUpload>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import FileUpload from "primevue/fileupload";
import Button from "primevue/button";
import Galleria from "primevue/galleria";

// define props
const props = defineProps<{
  title?: string;
  multiple?: boolean;
}>();

// define events
const emits = defineEmits<{
  (e: "onImageSelect", image: object): void;
}>();

const responsiveOptions: [
  {
    breakpoint: "1024px";
    numVisible: 5;
  },
  {
    breakpoint: "768px";
    numVisible: 3;
  },
  {
    breakpoint: "560px";
    numVisible: 1;
  }
];

const onSelectedFiles = (event) => {
  if (props.multiple) emits("onImageSelect", event.files);
  else emits("onImageSelect", event.files[0]);
};
</script>

<style scoped></style>
