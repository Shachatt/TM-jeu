<script setup>
import { ref } from 'vue'
const counter = ref(0)
</script>

<template>
  <h1>Hello World !</h1>
  <p>
    Je ne comprends pas grand chose
  </p>
  <button @click="counter++">
    Clique-moi ! ({{ counter }})
  </button>
</template>

<style scoped></style>
