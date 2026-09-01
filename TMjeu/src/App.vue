<script setup lang="ts">
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
const grille = GrilleVide() //on a notre grille mtn
//fonction pour se repérer dans la grille
function ligne(i:number) { 
    return Math.floor(i/4)
}
function colonne(i:number) {
    return i % 4
}
//fonction pour contrôler s'il y même valeur sur la colonne / ligne

function memeColonne(grille, index, let valeur:number){
    for (let i=0 ; i < 16 ; i += 4) { // comment faire pour dire que i c'est l'élé qu'on a choisi ?
        if ( valeur === grille[i.valeur]) {
            return false
        }
    }
    return true
}
function memeLigne(grille, index, let valeur:number){
    for (let  i = 0; i < 16 ; i++) {
        if ( valeur === grille[i.valeur]){
            return false
        }
    }
    return true
}

</script>

<template>
</template>

<style scoped>
</style>
