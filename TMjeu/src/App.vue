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

function melanger(tableau: number[]): number[] { // number dedans : le type dans le tableau, ext ce qui va en sortir
    const restants = [...tableau]   //le spread(...) consiste à étaler son contenu, ça crée une copie (car sinon c'est dans même case de stockage)
    const resultat: number[] = [] //si il y a : number[] c'est pour préciser que le résultat sera seulement un nombre ? et les deux [] pour dire que la case est actuellement vide ?
    while (restants.length > 0) { // le tableau des restants se vide, donc on continue de faire tant qu'il y aqlch dans le tableau
        const i = Math.floor(Math.random() * restants.length) //random trouve un décimal entre 0 et(*) la longueur restante, floor c'est pour arrondir vers la bas, 4-3-2-1 
        resultat.push(...restants.splice(i, 1))//push ajoute des éléments à la fin d'un tableau, splice enleve éé et return un tableau[] et les point servent à envoyer un number
    } //fonction générique de mélange de tableau
    return resultat //car c'est ce qu'on a besoin
}

function placerChiffre(index: number): boolean { // on rentre un chiffre et la fonction nous rend un boolean
    if (index >= 16) { //vérifie que les 16 (0 à 15) cases sont remplis, si oui on arrête tout
        return true
    }
    const caseCourante = grille[index] //c'est las case actuelle avec l'index dune case de notre grille
    if (!caseCourante) return false //vérifie si case existe, !qlch = si qlch est faux on return false, donc si ça existe pas, donc ça vérifie pour la suite
    const candidats = melanger([1, 2, 3, 4])//ici va utiliser la fonction pour recréer un tableau avec 1 2 3 et 4 mélangé. ça sort un tableau
    for (const candidat of candidats) { //for of est une bucle qui parcourt les valeurs du tableau, sans passer par index : "pour chacun des chiffres de la liste"
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

type Inequation = {case1:number, case2:number,signe:">"|"<"}
function grilleInequation(grille:Casevide[]){
    const signes: Inequation[]
    const caseCourante = grille[i] //c'est las case actuelle avec l'index dune case de notre grille
    if (!caseCourante) {return false}
    for (let i=0;i<16;i++){
        if (3 !== colonne(i)) {//côté de droite
            if (grille[i])?.valeur <= grille[i+1]?.valeur){
                signes.push("<")
            } else {
                signes.push(">")
            }
        }
        if (3!== ligne(i)) {
            if (grille[i]?.valeur <= grille[i+4]?.valeur){
                signes.push("<")
            } else {
                signes.push(">")
            }
        }
    } 
    return signes[]
}

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
console.log(melanger([1,2,3,4]))

</script>
<template>
</template>

<style scoped>
</style>