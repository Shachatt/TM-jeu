<script setup lang="ts">
  import { ref } from 'vue';
  const joueuractuel = ref('O');
  const plateau = ref([
    '', '', '',
    '', '', '',
    '', '', ''
  ]);
  const jouer = (index: number) => {
    if (plateau.value[index] === '') {
      plateau.value[index] = joueuractuel.value;
      joueuractuel.value = joueuractuel.value === 'O' ? 'X' : 'O';
    }
  }
import { computed } from 'vue';
const ligne1 = computed(() => plateau.value.slice(0, 3));
const ligne2 = computed(() => plateau.value.slice(0, 3));
const ligne3 = computed(() => plateau.value.slice(6, 9));

</script>

<template>
  <div class="plateau">
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
    <div class="ligne">
      <div class="case">?</div>
      <div class="case">?</div>
      <div class="case">?</div>
    </div>
  </div>
</template>

<style scoped>
.plateau {
  display: flex;
  flex-direction: column;
  gap: 5px;
  background-color: #f7b4d3;
  padding: 15px;
  border-radius: 10px;
}

.ligne {
  display: flex;
  gap: 5px;
}

.case {
  width: 100px;
  height: 100px;
  background-color: white;
  border: 2px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  font-weight: bold;
  cursor: pointer;
}
</style>
