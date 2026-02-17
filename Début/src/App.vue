<script setup>
import { ref } from 'vue'
const counter = ref(0)
</script>

<template>
  <h1>Hello World !</h1>
  <p>
    Je ne comprends pas grand chose
  </p>

<div class="buttons">
  <button @click="counter++">
    Clique-moi ! ({{ counter }})
  </button>

  <button @click="counter = 0">
    Réinitialiser
  </button>
</div>

</template>

<style scoped>

button {
  background-color: #144769;
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0f3752;
}

h1 {
  color: #D9B8C4;
  text-align: center;
}

p {
  color: #C499A9;
  text-align: center;
  font-style: italic;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 30px;
}
</style>
