<script setup lang="ts">
  import { ref, computed } from 'vue';
  const joueuractuel = ref('O');
  const plateau = ref([
    '', '', '',
    '', '', '',
    '', '', ''
  ]);
  const jouer = (index: number) => {
    if (plateau.value[index] === ''
      && jeutermine.value !== true
    ) {
      plateau.value[index] = joueuractuel.value;
      joueuractuel.value = joueuractuel.value === 'O' ? 'X' : 'O';
      verifiervictoire()
      
    }
  }

const ligne1 = computed(() => plateau.value.slice(0, 3));
const ligne2 = computed(() => plateau.value.slice(3, 6));
const ligne3 = computed(() => plateau.value.slice(6, 9));

const combinaisonsGagnantes = [
  [0, 1, 2], // ligne 1
  [3, 4, 5], // ligne 2
  [6, 7, 8], // ligne 3
  [0, 3, 6], // colonne 1
  [1, 4, 7], // colonne 2
  [2, 5, 8], // colonne 3
  [0, 4, 8], // diagonale 1
  [2, 4, 6]  // diagonale 2
];
const vainqueur = ref('');
const verifiervictoire = () => {
  combinaisonsGagnantes.forEach(element => {
    const element1 = element[0]!;
    const element2 = element[1]!;
    const element3 = element[2]!;
    if (plateau.value[element1] !== ''
    && plateau.value[element2] !== ''
    && plateau.value[element3] !== ''
    && plateau.value[element1] === plateau.value[element2] 
    && plateau.value[element1] === plateau.value[element3] 
    && plateau.value[element2] === plateau.value[element3]) {
      vainqueur.value = plateau.value[element1] === 'O' ? 'joueur1' : 'joueur2'
      jeutermine.value = true
    }
  });
}

const jeutermine = ref(false)
const reinitialiser = () => {
  location.reload();
}

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

  <div
  v-if="vainqueur !== ''">
      <p>VICTOIRE {{ vainqueur }}</p>
      <button @click="reinitialiser"> Réinitialiser
      </button>
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

p {
  color: #e6bfd1;
  font-style: normal;
}
</style>
