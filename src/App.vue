<template>
  <main class="container">
    <h1>Running TensorFlowJS in version : {{ version }}</h1>
    <div class="predict-container"></div>
  </main>
</template>

<script setup>
import * as tf from '@tensorflow/tfjs'
import * as tfjsWasm from '@tensorflow/tfjs-backend-wasm'
import * as tfjsCpu from '@tensorflow/tfjs-backend-cpu'
import * as tfjsWebGL from '@tensorflow/tfjs-backend-webgl'
import toBee from './assets/tobee.png'

tfjsWasm.setWasmPaths(
  `https://cdn.jsdelivr.net/npm/@tensorflow/tfjs-backend-wasm@${tfjsWasm.version_wasm}/dist/`
)

import { onMounted, ref } from '@vue/runtime-core'

const version = ref(tf.version_core)

const model = ref()
const matrix = ref([])

const getPredictions = async () => {
  const example = tf.randomNormal([1, 100])
  // const example = tf.tensor([1,100], [1, 100])
  // console.log(example);

  const returnTensor = false
  const predictions = await model.value.predict(example, returnTensor)

  console.log(predictions);

  //If predictions isn't null, do something
  // if (predictions.length > 0) {
  //   //Do something for each predictions in the array
  //   for (let i = 0; i < predictions.length; i++) {
  //     console.log(predictions[i]);
  //   }
  // }
}

onMounted(async () => {
  await tf.ready()
  model.value = await tf.loadLayersModel('/models/dcgan_MNIST/model.json');
  getPredictions()
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
