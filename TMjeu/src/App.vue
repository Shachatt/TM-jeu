<script setup lang="ts">
import { ref, } from 'vue';
// grile vide mais l'index est comme un trait
type Casevide = {valeur:number|null, verrouille:boolean} //comprend que valeur peut etre null ou9 un nombre
function GrilleVide(): Casevide[] { //comprendre que les cases acceptent valeur accepte null et nmber
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

function melanger(tableau: number[]): number[] { // pourquoi il y a deux fois number[]
    const restants = [...tableau]   //je n'ai as compris pour tableau est écrit entre [] et avec les trois points devant
    const resultat: number[] = [] //si il y a : number[] c'est pour préciser que le résultat sera seulement un nombre ? et les deux [] pour dire que la case est actuellement vide ?
    while (restants.length > 0) { // le tableau des restants se vide, donc on continue de faire tant qu'il y aqlch dans le tableau
        const i = Math.floor(Math.random() * restants.length) // a quel moment ça sait que c'est des chiffres entre 1 et 4 ?, et je ne comprends pas vrmt le calcul, il y a quoi dans le math random ?
        resultat.push(...restants.splice(i, 1))//peux-tu me réexpliquer push et les 3 points
    }
    return resultat //car c'est ce qu'on a besoin
}

function placerChiffre(index: number): boolean { // pourquoi le :boolean n'est pas dans la parenthese ?
    if (index >= 16) { //vérifie que les 15 cases sont remplis, si oui on arrête tout
        return true
    }
    const caseCourante = grille[index] //c'est las case actuelle avec l'index dune case de notre grille
    if (!caseCourante) return false //vérifie si case existe, ! = si existe, et si ça existe pas c'est faux, donc ça vérifie pour la suite
    const candidats = melanger([1, 2, 3, 4])//ici va utiliser la fonction pour recréer un tableau avec 1 2 3 et 4 mélangé. mais ça en sort qu'un élément ?
    for (const candidat of candidats) { //c'est quoi for (_ of _) ? fin le return fini la boucle ? jsp j'ai pas trop compris
        if (peutPlacer(grille, index, candidat)) {
            caseCourante.valeur = candidat // if true, alors on peut écrire la valeur dans la case
            if (placerChiffre(index + 1)) { // si la case d'après renvoie aussi true alors c'est nice. RECURSIVITE la fonction se relance pour les case suivantes
                return true
            }
            caseCourante.valeur = null // si la case suivant ne fonctionne pas, on efface le chiffre
        }
    }

    return false //si les quatres on échoué valeur on échoué on retourne false après avoir effacé la valeur de la case
}
//function placerChiffreancien(i:number){
  //  let chiffres = sort.[1;2;3;4]
 //   if (i === 16){  //si tous les index sont passés alors case remplies et juste
   //     return true
   // } else {
//        const val = grille[i]
  //          if (!val) return false
    //        grille.valeur = val //je ne comprend pas vraiment cette ligne
 //           return false
   //     } 
   // for n of grille[] {
//    }
//}
    
const testgrille = [{ valeur: null, verrouille: false}, { valeur: 2, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false},
        { valeur: 3, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}, { valeur: 2, verrouille: true},
        { valeur: null, verrouille: false}, { valeur: 1, verrouille: true}, { valeur: null, verrouille: false}, { valeur: null, verrouille: false}
    ]
const reussi = placerChiffre(0)
console.log(peutPlacer(testgrille, 9, 4))
console.log(peutPlacer(testgrille, 9, 3))
console.log(peutPlacer(testgrille, 9, 1))
console.log(reussi, grille)

</script>
<template>
</template>

<style scoped>
</style>