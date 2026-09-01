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

function pasmemeColonne(grille:{valeur:number|null;verrouille:boolean}[],index:number, valeur:number){
    for (let i=colonne(index) ; i < 16 ; i += 4) { // comment faire pour dire que i c'est l'élé qu'on a choisi ?
        if ( valeur === grille[i].valeur) {
            return false
        }
    }
    return true
}
function pasmemeLigne(grille:{valeur:number|null;verrouille:boolean}[],index:number, valeur:number){
    for (let  i = ligne(index)*4 ; i < ligne(index)*4+4 ; i++) {
        if ( valeur === grille[i].valeur){
            return false
        }
    }
    return true
}
function peutPlacer(grille:{valeur:number;verrouille:boolean}[],index:number, valeur:number){
    return pasmemeColonne(grille,index,valeur) && pasmemeLigne(grille,index,valeur) 
}

</script>

<template>
</template>

<style scoped>
</style>