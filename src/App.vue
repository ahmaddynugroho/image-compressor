<template>
  <div class="container mx-auto my-16 max-w-md">
    <a
      href="https://github.com/ahmaddynugroho/image-compressor-app"
      class="text-gray-500 text-4xl font-thin uppercase text-center my-8 block"
      >Image Compressor</a
    >
    <label
      for="file"
      class="border rounded border-gray-700 my-2 px-4 py-2 text-gray-400 bg-gray-800 cursor-pointer block min-w-max hover:bg-gray-700 hover:text-gray-300"
      >{{ fileName }}</label
    >
    <p v-if="!compressedLink" class="font-thin text-gray-400 mt-2">
      Compression Quality:
    </p>
    <input
      v-if="!compressedLink"
      class="border rounded border-gray-700 my-2 px-4 py-2 text-gray-400 bg-gray-800 hover:bg-gray-700 hover:text-gray-300"
      id="quality"
    />
    <a v-if="compressedLink" :href="compressedLink">download</a>
    <input @change="compress($event)" type="file" id="file" accept="image/*" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import Compressor from "compressorjs";

export default defineComponent({
  setup() {
    // data
    const compressedLink = ref("");
    const fileName = ref("Browse an image to triggers compression...");

    // methods
    const compress = (e: any) => {
      // something
      const file = e.target.files[0];
      fileName.value = file.name;
      console.log((<HTMLInputElement>document.querySelector("#quality")).value);

      if (!file) return;

      new Compressor(file, {
        quality: Number(
          (<HTMLInputElement>document.querySelector("#quality")).value
        ),
        success(result: any) {
          console.log(e.target.files[0]);
          console.log("Compress success", result.name);
          console.log(result);
          compressedLink.value = `${window.URL.createObjectURL(result)}`;
        },
        error(err) {
          console.log(err.message);
        },
      });
    };

    return { compress, compressedLink, fileName };
  },
});
</script>

<style scoped>
#file {
  opacity: 0;
  position: absolute;
  z-index: -1;
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
  display: block;
  min-width: 100%;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
  display: block;
  min-width: 100%;
}
</style>
