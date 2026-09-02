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
type Casevide = {valeur:number|null, verrouille:boolean}
//fonction pour se repérer dans la grille
function ligne(i:number) { 
    return Math.floor(i/4)
}
function colonne(i:number) {
    return i % 4
}
//fonction pour contrôler s'il y même valeur sur la colonne / ligne
function pasmemeColonne(grille:Casevide[],index:number, valeur:number){
    for (let i=colonne(index) ; i < 16 ; i += 4) { 
        if ( valeur === grille[i]?.valeur) { //grille[i].valeur : prend la valeur de ce qu'il y a à cet index : ?. permet d'éviter l'undefind
            return false
        }
    }
    return true
}
function pasmemeLigne(grille:Casevide[],index:number, valeur:number){
    for (let  i = ligne(index)*4 ; i < ligne(index)*4+4 ; i++) {
        if ( valeur === grille[i]?.valeur){
            return false
        }
    }
    return true
}
function peutPlacer(grille:Casevide[],index:number, valeur:number){
    return pasmemeColonne(grille,index,valeur) && pasmemeLigne(grille,index,valeur) 
}
const testgrille = [{ valeur: null, verrouille: false}, { valeur: 2, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: 3, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: 2, verrouille: true},
        { valeur: null, verrouille: false}, { valeur: 1, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}
    ]

console.log(peutPlacer(testgrille, 9, 4))
</script>
<template>
</template>

<style scoped>
</style>