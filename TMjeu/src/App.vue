<script setup lang="ts">
import { ref } from 'vue';
function GrilleVide() {
    return [ 
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}
    ]
    }
function Inequations() {
    const ValeurA = ref({ligne: 0, colonne: 0});
    const ValeurB = ref({ligne: 0, colonne: 1});
    const inequ = ref({de: ValeurA, vers: ValeurB, signe: ''>''});
    
    if ((ValeurA.value) >= (ValeurB.value)) {
        return {correct: true}
    }
    else if ((ValeurA.value) <= (ValeurB.value)) {
        return {correct: false}
    }
    }

</script>

<template>
</template>

<style scoped>
</style>
