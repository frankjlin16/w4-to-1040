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
  <div class="container p-4">
    <h1 class="text-xl">Image to Text</h1>
    <UContainer class="m-4 w-1/2">
      <UFormGroup label="Image URL" class="mb-4">
        <UInput
          placeholder="https://example-image.com/sample.png"
          v-model="imageUrl"
        />
      </UFormGroup>
      <div class="flex flex-row-reverse">
        <UButton @click="scan">Scan</UButton>
      </div>
    </UContainer>
    <UCard class="m-4">
      <p v-if="scanning">Scanning...</p>
      <p v-else>{{ text }}</p>
    </UCard>
  </div>
</template>
