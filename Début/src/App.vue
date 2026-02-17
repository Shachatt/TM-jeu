<script setup>
import { ref } from 'vue'
const counter = ref(0)
</script>

<template>
  <h1>Hello World !</h1>
  <p>
    Je ne comprends pas grand chose
  </p>

<p>
  compteur : {{ counter }}
</p>
<div class="buttons">
  
  <button @click="counter -=10" class="button-minus"> -10 </button>
  <button @click="counter -=5" class="button-minus"> -5 </button>
  <button @click="counter--" class="button-minus"> -1 </button>
  <button @click="counter++" class="button-plus"> +1 </button>
  <button @click="counter +=5" class="button-plus"> +5 </button>
  <button @click="counter +=10" class="button-plus"> +10 </button>
  <button @click="counter = 0" class="button-reset"> Réinitialiser </button>
</div>

</template>

<style scoped>

.button-minus {
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
.button-minus:hover {
  background-color: #0f3752;
}

.button-plus {
  background-color: #e6c92a;
  color: rgb(105, 103, 103);
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}
.button-plus:hover {
  background-color: #c29312;
}
.button-reset {
  background-color: #7de74c;
  color: rgb(0, 0, 0);
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}
.button-reset:hover {
  background-color: #59a314;
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
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
</style>
