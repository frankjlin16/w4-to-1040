<script setup lang="ts">
import { createWorker } from "tesseract.js";

const title = ref("W2 to 1040");
const text = ref("Nothing to see here...");
const scanning = ref(false);
const imageUrl = ref("");

async function scan() {
  scanning.value = true;
  const worker = await createWorker("eng");
  const ret = await worker.recognize(imageUrl.value);
  text.value = ret.data.text;
  await worker.terminate();
  scanning.value = false;
}
</script>

<template>
  <Head>
    <Title>{{ title }}</Title>
  </Head>
  <div>
    <h1>Image to Text</h1>
    <input type="text" v-model="imageUrl">
    <button @click="scan">Scan</button>
    <p v-if="scanning">Scanning...</p>
    <p v-else>{{ text }}</p>
  </div>
</template>
