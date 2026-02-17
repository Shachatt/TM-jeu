<script setup lang="ts">
  import { ref, computed } from 'vue';
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

const ligne1 = computed(() => plateau.value.slice(0, 3));
const ligne2 = computed(() => plateau.value.slice(3, 6));
const ligne3 = computed(() => plateau.value.slice(6, 9));

</script>

<template>
  <div class="plateau">
    <!-- Ligne 1 -->
    <div class="ligne">
      <div
        v-for="(cell, index) in ligne1"
        :key="index"
        class="case"
        :class="{ 'joueur1': cell === 'O', 'joueur2': cell === 'X'}"
        @click="jouer(index)"
      >
        {{ cell }}
      </div>
    </div>
    <!-- Ligne 2 -->
    <div class="ligne">
      <div
        v-for="(cell, index) in ligne2"
        :key="index"
        class="case"
        :class="{ 'joueur1': cell === 'O', 'joueur2': cell === 'X'}"
        @click="jouer(index + 3)"
      >
        {{ cell }}
      </div>
    </div>
    <!-- Ligne 3 -->
    <div class="ligne">
      <div
        v-for="(cell, index) in ligne3"
        :key="index"
        class="case"
        :class="{ 'joueur1': cell === 'O', 'joueur2': cell === 'X'}"
        @click="jouer(index + 6)"
      >
        {{ cell }}
      </div>
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

.joueur1 {
  color: rgb(69, 55, 189);
}

.joueur2 {
  color: rgb(214, 49, 165);
}
</style>
