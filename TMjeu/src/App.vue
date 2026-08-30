import { ref } from 'vue';
// grile vide mais l'index est comme un trait
function GrilleVide() { 
    return [ 
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}
    ]
    }
//fonction pour se repérer dans la grille
function colonne(i:number) {
    return i % 4
}

function ligne(i:number) { 
    Math.floor(i/4)
    return
}
</script>

<template>
</template>

<style scoped>
</style>
