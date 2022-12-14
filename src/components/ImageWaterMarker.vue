<template>
  <div>
    <input
      ref="files"
      type="file"
      name="image"
      id="image"
      @input="fileInputed"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import watermark from "watermarkjs";

// states
const files = ref(null);
const output = ref(null);

const fileInputed = () => {
  const imageFile = files.value.files[0];
  const fileName = imageFile.name;

  watermark([imageFile, "./img/softzino.png"])
    .image(watermark.image.lowerLeft(0.5))
    .then((image) => {
      downloadBlob(image.src, fileName);
    });
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
</script>

<style lang="scss" scoped>
.image-container {
  height: 500px;
  width: 600px;
}
</style>
