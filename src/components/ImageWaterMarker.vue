<template>
  <div class="w-full">
    <!-- <Button @click="handleFileInput" label="Select Folder" /> -->
    <div class="grid grid-nogutter surface-section text-800">
      <div
        class="col-12 md:col-6 p-6 text-center md:text-left flex align-items-center"
      >
        <section>
          <span class="block text-6xl font-bold mb-1"> Watermark Adder </span>
          <div class="text-6xl text-primary font-bold mb-3">
            Add Watermark on your image
          </div>
          <p class="mt-0 mb-4 text-700 line-height-3">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </p>

          <Button
            label="Learn More"
            type="button"
            class="mr-3 p-button-raised"
          />
          <Button label="Live Demo" type="button" class="p-button-outlined" />
        </section>
      </div>
      <div class="col-12 md:col-6 overflow-hidden">
        <img
          src="images/blocks/hero/hero-1.png"
          alt="Image"
          class="md:ml-auto block md:h-full"
          style="clip-path: polygon(8% 0, 100% 0%, 100% 100%, 0 100%)"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import watermark from "watermarkjs";
import Button from "primevue/button";

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
