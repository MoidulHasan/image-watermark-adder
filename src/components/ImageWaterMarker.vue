<template>
  <div class="w-full">
    <!-- <Button @click="handleFileInput" label="Select Folder" /> -->
    <div class="grid grid-nogutter surface-section text-800">
      <div
        class="col-12 md:col-6 px-6 text-center md:text-left flex align-items-center"
      >
        <section>
          <span class="block text-6xl font-bold mb-1"> Watermark Adder </span>
          <div class="text-6xl text-primary font-bold mb-3">
            Add Watermark on your image
          </div>
        </section>
      </div>
    </div>

    <div class="grid">
      <div
        class="col-12 md:col-4 px-6 text-center md:text-left flex align-items-center"
      >
        <div>
          <WatermarkImageUpload
            class="w-full"
            title="Upload Markdown Image"
            @onImageSelect="watermarkImageUploadHandler"
            :watermarkedImage="imageToWaterMark"
          />
          <WatermarkImageUpload
            title="Upload Image To add Markdown"
            :multiple="true"
            @onImageSelect="ImageUploadHandler"
            class="mt-2"
          />
        </div>
      </div>
      <div
        class="col-12 md:col-4 px-6 text-center md:text-left flex align-items-center"
      ></div>
      <div class="col-12 md:col-4 px-6 text-center md:text-left">
        <PreviewImage :file="ImageForPreview" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, ref } from "vue";
import watermark from "watermarkjs";
import { useToast } from "primevue/usetoast";
import WatermarkImageUpload from "./WatermarkImageUpload.vue";
import PreviewImage from "./PreviewImage.vue";
// hooks
const toast = useToast();

// states
const imageToWaterMark = ref(null);
const watermarkedImage = ref(null);
const ImageForPreview = ref(null);

// methods
const watermarkImageUploadHandler = (image) => {
  watermarkedImage.value = image;
};

const ImageUploadHandler = (image) => {
  console.log(image);
  if (image?.length > 1) ImageForPreview.value = image[0];

  imageToWaterMark.value = image;
};

const fileInputed = async (image) => {
  const imageFile = image;
  const fileName = imageFile.name;
  watermark([imageFile, imageToWaterMark.value.objectURL])
    .image(watermark.image.lowerLeft(0.5))
    .then((image) => {
      downloadBlob(image.src, fileName);
    });

  // watermark([imageFile, "./img/softzino.png"])
  //   .image(watermark.image.lowerLeft(0.5))
  //   .then((image) => {
  //     downloadBlob(image.src, fileName);
  //   });
};

const downloadBlob = (blobUrl, name = "file.png") => {
  const link = document.createElement("a");
  link.href = blobUrl;
  link.download = name;

  document.body.appendChild(link);

  link.dispatchEvent(
    new MouseEvent("click", {
      bubbles: true,
      cancelable: true,
      view: window,
    })
  );

  document.body.removeChild(link);
};

const handleFileInput = async (evt) => {
  const out = {};
  const dirHandle = await showDirectoryPicker();
  await handleDirectoryEntry(dirHandle, out);
  console.log(out);
};

const handleDirectoryEntry = async (dirHandle, out) => {
  for await (const entry of dirHandle.values()) {
    if (entry.kind === "file") {
      const file = await entry.getFile();
      out[file.name] = file;
    }
    if (entry.kind === "directory") {
      const newHandle = await dirHandle.getDirectoryHandle(entry.name, {
        create: false,
      });
      const newOut = (out[entry.name] = {});
      await handleDirectoryEntry(newHandle, newOut);
    }
  }
};
</script>

<style lang="scss" scoped>
.image-container {
  height: 500px;
  width: 600px;
}
</style>
