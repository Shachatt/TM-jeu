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
  <button @click="counter -=10" class=" button-pos button-minus btn-bottom-right" style="margin-top: 50px;"> -10 </button>
  <button @click="counter -=5" class="button-pos button-minus btn-bottom-right" style="margin-top: 100px"> -5 </button>
  <button @click="counter--" class="button-pos button-minus btn-top-right"> -1 </button>
  <button @click="counter++" class="button-pos button-plus btn-top-left"> +1 </button>
  <button @click="counter +=5" class="button-pos button-plus btn-bottom-left" style="margin-top: 50px;"> +5 </button>
  <button @click="counter +=10" class="button-pos button-plus btn-bottom-left" style="margin-top: 100px"> +10 </button>
  <button @click="counter = 0" class="button-pos button-reset btn-center"> Réinitialiser </button>
</div>
</template>

<style scoped>

.button-pos {
  position: absolute;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.button-minus {
  background-color: #144769;
  color: white;
}
.button-minus:hover {
  background-color: #0f3752;
}

.button-plus {
  background-color: #e6c92a;
  color: rgb(105, 103, 103);
}
.button-plus:hover {
  background-color: #c29312;
}
.button-reset {
  background-color: #7de74c;
  color: rgb(0, 0, 0);
  }
.button-reset:hover {
  background-color: #59a314;
}

.btn-top-left {
  top: 20px;
  left: 20px;
}

.btn-top-right {
  top: 20px;
  right: 20px;
}

.btn-bottom-left {
  bottom: 20px;
  left: 20px;
}

.btn-bottom-right {
  bottom: 20px;
  right: 20px;
}

.btn-center {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
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
