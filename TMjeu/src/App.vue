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

function placerChiffre(grille:Casevide[],index: number): boolean { // on rentre un chiffre et la fonction nous rend un boolean
    if (index >= 16) { //vérifie que les 16 (0 à 15) cases sont remplis, si oui on arrête tout
        return true
    }
    const caseCourante = grille[index] //c'est las case actuelle avec l'index dune case de notre grille
    if (!caseCourante) {return false} //vérifie si case existe, !qlch = si qlch est faux on return false, donc si ça existe pas, donc ça vérifie pour la suite
    const candidats = melanger([1, 2, 3, 4])//ici va utiliser la fonction pour recréer un tableau avec 1 2 3 et 4 mélangé. ça sort un tableau
    for (const candidat of candidats) { //for of est une bucle qui parcourt les valeurs du tableau, sans passer par index : "pour chacun des chiffres de la liste"
        if (peutPlacer(grille, index, candidat)) {
            caseCourante.valeur = candidat // if true, alors on peut écrire la valeur dans la case
            if (placerChiffre(grille,index + 1)) { // si la case d'après renvoie aussi true alors c'est nice. RECURSIVITE la fonction se relance pour les case suivantes
                return true
            }
            caseCourante.valeur = null // si la case suivant ne fonctionne pas, on efface le chiffre
        }
    }

    return false //si les quatres on échoué valeur on échoué on retourne false après avoir effacé la valeur de la case
}

type Inequation = {case1:number, case2:number,signe:">"|"<"}
function grilleInequation(grille:Casevide[]){
    const InequationComplete: Inequation[] = []//tableau vide
    for (let i=0;i<16;i++){
        const valgrille = grille[i]?.valeur
        if (valgrille==null){throw new Error("case vide")}
        if (3 !== colonne(i)) {//côté de droite
            const valAcomparerCote = grille[i+1]?.valeur
               if (valAcomparerCote==null){throw new Error("case vide")}
            if (valgrille < valAcomparerCote){ 
                InequationComplete.push({case1:i, case2:i+1,signe:"<"}) 
            } else {
                InequationComplete.push({case1:i, case2:i+1,signe:">"})
            }
        }
        if (3!== ligne(i)) {
            const valAcomparerDessous = grille[i+4]?.valeur
                if (valAcomparerDessous==null){throw new Error("case vide")}
            if (valgrille < valAcomparerDessous){
                InequationComplete.push({case1:i, case2:i+4,signe:"<"})
            } else {
                InequationComplete.push({case1:i, case2:i+4,signe:">"})
            }
        }
    } 
    return InequationComplete
}
function testerInequation(grille:Casevide[], liste:Inequation[],index:number): boolean{
    for (const InequationComplete of liste){
        if (InequationComplete.case1 !== index && InequationComplete.case2 !== index){
            continue}
        const val1 = grille[InequationComplete.case1]?.valeur
        const val2 = grille[InequationComplete.case2]?.valeur
        if (val1 == null || val2 == null){
            continue
        } 
        if (">" === InequationComplete.signe && val1 < val2){
            return false
        }
        if ("<" === InequationComplete.signe && val1 > val2){
            return false
        }
    }
    return true
}
function unicite(grille:Casevide[],liste:Inequation[],index:number){
    let solution = 0
    if (index >= 16){
        return 1
    }
    const caseCourante = grille[index]
    if (!caseCourante) {return 0}
    if (caseCourante.verrouille){return unicite(grille,liste,index+1)}
    const candidats = melanger([1,2,3,4])
    for (const candidat of candidats){
        if (peutPlacer(grille,index,candidat)){ 
            caseCourante.valeur = candidat //écrire la valeur dedans
            if (testerInequation(grille,liste,index)){
                solution += unicite(grille, liste, index + 1) 
            }
        }
        caseCourante.valeur = null
        if (solution >= 2){
            break
        }
    }
    return solution
}
function enleverEle(grille:Casevide[], liste:Inequation[]):void{ //le void signifie que la fonction renvoie rien, va juste modifié la grille et tableau
    for (const CaseMtn of grille ){ //On verrouille toutes les cases de la grille comme ça pas de changement dans notre grille initial
        CaseMtn.verrouille = true 
    }
    const indices:number[] = [] //on va construire une grille de 0 à 15 et ensuite la mélanger pour les index
    for (let i = 0; i < grille.length ; i++){//ça revient à ma question d'avant.
        indices.push(i) //utiliser push pour ajouter un élé au tableau vide
    }
    const indicesMelanges = melanger(indices)
    // ensuite on va retirer les chiffres et faire attention si problème d'unicité
    for (const index of indicesMelanges){ 
        const caseCourante = grille[index] //on parle de la case à cet index (donc valeur et verrouille)
        if (!caseCourante) continue
        const ancienneValeur = caseCourante.valeur //l'ancienne valeur est celle qu'on va essayer d'enlever car si unicité ne va pas on peut la remettre
        caseCourante.valeur = null
        caseCourante.verrouille = false
        if (unicite(grille,liste,0) !== 1){  //s'il n'existe pas qu'une solution alors il faut pas retirer la valeur
            caseCourante.valeur = ancienneValeur
            caseCourante.verrouille = true //ça nous permet de retirer l'enlevement du chiffre
        }
    }
    //ici on retire inéquation
    for (let k = liste.length - 1; k >= 0; k--){
        const retiree = liste.splice(k, 1)[0]  //donc rend un tableau et [0] est le premier élé
        if (!retiree) continue
        if (unicite(grille,liste,0) !== 1){
            liste.splice(k, 0, retiree) //k va supprimer 0 élé à la position k et insère le retiree (donc on remet l'inéquation)
        }
    }
}

const reussi = placerChiffre(grille,0)
const Listeineq = grilleInequation(grille)
console.log(reussi, grille)//la grille avec toute les valeurs placés
console.log(grille)
console.log(Listeineq)
enleverEle(grille,Listeineq)
console.log(unicite(grille,Listeineq,0))
console.log(Listeineq)

</script>

<template>
</template>

<style scoped>
</style>