<script setup lang="ts">
import { ref, computed } from 'vue';
    // grile vide mais l'index est comme un trait
type Casevide = {valeur:number|null, verrouille:boolean} //comprend que valeur peut etre null ou9 un nombre
function GrilleVide(n:number): Casevide[] { //comprendre que les cases acceptent valeur accepte null et nmber
    const arr : Casevide[]=[]
    for (let i=0;i<n*n;i++){   //for (d'où on part ; jusqu'où on va ; quel jump on fait)
        arr.push({ valeur: null, verrouille: false})
    }
    return arr
    }

const tailleGrille = ref<number>(4) //taille initial de la grille

function creerListeNb(){
    const listeTailleGrille: number[] = []
    for (let i = 1; i < tailleGrille.value + 1; i++){ //comme ça on pousse tous les chiffres de 1 à la taille qu'on a besoin dans une liste
        listeTailleGrille.push(i)
    }
    return listeTailleGrille
}
const ListeTailGril = computed (() => creerListeNb()) //computed se calcule tout seul

   //fonction pour se repérer dans la grille
function ligne(i:number) { 
    return Math.floor(i/tailleGrille.value) //math.floor évite les virgules
}
function colonne(i:number) {
    return i % tailleGrille.value
}
    //fonction pour contrôler s'il y même valeur sur la colonne / ligne
function pasmemeColonne(grille:Casevide[],index:number, valeur:number){
    for (let i=colonne(index) ; i < tailleGrille.value*tailleGrille.value ; i += tailleGrille.value) { 
        if (i === index){continue}
        if ( valeur === grille[i]?.valeur) { //grille[i].valeur : prend la valeur de ce qu'il y a à cet index : ?. permet d'éviter l'undefind
            return false
        }
    }
    return true
}
function pasmemeLigne(grille:Casevide[],index:number, valeur:number){
    for (let  i = ligne(index)*tailleGrille.value ; i < ligne(index)*tailleGrille.value+tailleGrille.value ; i++) {
        if (i === index){continue}
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
    const resultat: number[] = [] //si il y a : number[] c'est pour préciser que le résultat sera seulement un nombre et les deux [] pour un tableau vide
    while (restants.length > 0) { // le tableau des restants se vide, donc on continue de faire tant qu'il y aqlch dans le tableau
        const i = Math.floor(Math.random() * restants.length) //random trouve un décimal entre 0 et(*) la longueur restante, floor c'est pour arrondir vers la bas, 4-3-2-1 
        resultat.push(...restants.splice(i, 1))//push ajoute des éléments à la fin d'un tableau, splice enleve éé et return un tableau[] et les point servent à envoyer un number
    } //fonction générique de mélange de tableau
    return resultat //car c'est ce qu'on a besoin
}

function placerChiffre(grille:Casevide[],index: number): boolean { // on rentre un chiffre et la fonction nous rend un boolean
    if (index >= tailleGrille.value*tailleGrille.value) { //vérifie que les 16 (0 à 15) cases sont remplis, si oui on arrête tout dépend taille de grille
        return true
    }
    const caseCourante = grille[index] //c'est las case actuelle avec l'index dune case de notre grille
    if (!caseCourante) {return false} //vérifie si case existe, !qlch = si qlch est faux on return false, donc si ça existe pas, donc ça vérifie pour la suite
    const candidats = melanger(ListeTailGril.value)//ici va utiliser la fonction pour recréer un tableau avec 1 2 3 et 4 mélangé, avec les chiffre jusqu'à taille de grille. ça sort un tableau ATTENTION comment je fais pour le rendre muable au chgmt de taille de grille
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
    // Crée les inéquation en fonction de la grille
function grilleInequation(grille:Casevide[]){
    const InequationComplete: Inequation[] = []//tableau vide
    for (let i=0;i<tailleGrille.value*tailleGrille.value;i++){
        const valgrille = grille[i]?.valeur
        if (valgrille==null){throw new Error("case vide")}
        if (tailleGrille.value-1 !== colonne(i)) {//côté de droite
            const valAcomparerCote = grille[i+1]?.valeur
            if (valAcomparerCote==null){throw new Error("case vide")}
            if (valgrille < valAcomparerCote){ 
                InequationComplete.push({case1:i, case2:i+1,signe:"<"}) 
            } else {
                InequationComplete.push({case1:i, case2:i+1,signe:">"})
            }
        }
        if (tailleGrille.value-1!== ligne(i)) { //case en dessous
            const valAcomparerDessous = grille[i+tailleGrille.value]?.valeur
                if (valAcomparerDessous==null){throw new Error("case vide")}
            if (valgrille < valAcomparerDessous){
                InequationComplete.push({case1:i, case2:i+tailleGrille.value,signe:"<"})
            } else {
                InequationComplete.push({case1:i, case2:i+tailleGrille.value,signe:">"})
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
        if (val1 == null || val2 == null){ //si on compare une case avec une pleine ça rendra faux
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
    if (index >= tailleGrille.value*tailleGrille.value){
        return 1
    }
    const caseCourante = grille[index]
    if (!caseCourante) {return 0}
    if (caseCourante.verrouille){return unicite(grille,liste,index+1)}
    const candidats = melanger(ListeTailGril.value) //regarder comment faire pour avoir liste de nombre de la taille d'un coté de grille
    for (const candidat of candidats){
        if (peutPlacer(grille,index,candidat)){ 
            caseCourante.valeur = candidat //écrire la valeur dedans si seulement ça convient
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
    const indices:number[] = [] //on va construire une grille de 0 à taille de grille et ensuite la mélanger pour les index
    for (let i = 0; i < grille.length ; i++){
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
    // génération de tableau vide, remplit avec nouveau jeu
const grilleDeJeu = ref<Casevide[]>([])
const ineqDeJeu = ref<Inequation[]>([])
    // ici cela crée la grille pour laffichage en incluant chiffre et ineq, mais toujours de mnière variable
const tailleAffGrille = computed(() => 2 * tailleGrille.value - 1) //d'abord en double et on en soustrait un
    // on crée la liste des emplacement entre case
type Endroit = {forme: 'carre', index:number} | {forme: 'vertical', haut:number, bas:number} | {forme: 'horizontal', gauche:number, droite:number} | {forme:'espace'} //les cases ont l'index de 0-15

const endroits = computed(() => {
    const endroitB: Endroit[] = []
    for (let lign = 0; lign < tailleAffGrille.value; lign++){ //enft script setup est déjà le corps d'une fonction donc va exécuter tout de haut en bas
        for (let colonn = 0; colonn < tailleAffGrille.value; colonn++){ //1ere for passe dans les lignes et ensuite 2eme colonne des lignes, ne s'enregistre pas 
            if (lign % 2 === 0 && colonn % 2 === 0){ //la c'est pour que les deux valeurs soient paires c'est les carrés
                endroitB.push({forme:'carre', index:tailleGrille.value * (lign/2) + (colonn/2)}) //on y inclus l'index pour la petite grille carré, pas de problème car toujours chiffre pair -> %2
            } else {
                if (lign%2!==0 && colonn%2===0){
                    const idx1 = tailleGrille.value * ((lign-1)/2) + (colonn/2)
                    const idx2 = idx1 + tailleGrille.value
                    endroitB.push({forme: 'vertical', haut:idx1, bas:idx2})
                } else if (lign%2===0 && colonn%2!==0){
                    const idx1 = tailleGrille.value * (lign/2) + ((colonn - 1)/2) //donc on garde le même style que dans carré, la constante est-elle au bonne endroit ?
                    const idx2 = tailleGrille.value * (lign/2) + ((colonn + 1)/2) 
                    endroitB.push({forme: 'horizontal', gauche:idx1, droite:idx2})
                } else {
            endroitB.push({forme: 'espace'}) }
            }
        }
    }
    return endroitB
})

function trouverInequation (endroit: Endroit){
    if (endroit.forme === 'horizontal') {
        return ineqDeJeu.value.find(item => item.case1 === endroit.gauche && item.case2 === endroit.droite)  // find permet de parcourir un tabelau et de s'arreter sur ele qui respecte ()  
    }
    if (endroit.forme === 'vertical'){
        return ineqDeJeu.value.find(item => item.case1 === endroit.haut && item.case2 === endroit.bas)        
    }
    return undefined
}

function trouverSigne(endroit: Endroit) {
        // 1. Vérifier si c'est horizontal
    return trouverInequation(endroit)?.signe
}

    // une piste est un segment compris entre deux lignes ou deux colonnes
    // const pistes = `repeat(${tailleGrille-1}, 3rem 1rem) 3rem` 
const pistes = computed(() => `repeat(${tailleGrille.value - 1}, 3rem 2rem) 3rem`)
    //permet de déclarer la piste il y en a 7, mais je capte pas vraiment comment çA se crée, les rem est une unité relative à la taille de police
    // répète 3 x (3x16px), repete toute la liste donc 3x une liste de 3 et 1 et le dernier c'est trois
    // `` crée une chaine qui passe à la ligne et le résultat fonction ${} est une chiane de caractère et ${} disparait

const grilleFinie = computed(() => { //pour voir si les grille est complète utilisé dans autre fonction
    for (let i = 0 ; i < tailleGrille.value*tailleGrille.value ; i++){
        const valeurActive = grilleDeJeu.value[i]?.valeur
        if (valeurActive == null){return false}
        if (!peutPlacer(grilleDeJeu.value,i,valeurActive) || !testerInequation(grilleDeJeu.value,ineqDeJeu.value,i)){
        return false 
        } }
        return true
});

const caseActive = ref<number | null>(null)
function rendreActif (index:number){
    const caseActu = grilleDeJeu.value[index]
    if (!caseActu) return
    if (grilleFinie.value) return //pour ne plus écrire lorsque que la grille est remplie
    if (caseActu.verrouille === false) { //donc toutes les cases modifiables
        if (caseActive.value !== index){
            caseActive.value = index
        } else {
            caseActive.value = null //pour enlever la séléction si déjà selectionné
        }
    }
}
function ecrireValeur (valeur:number|null){
    if (caseActive.value === null) return //on écrit que dans case active
    const caseAEcrire = grilleDeJeu.value[caseActive.value]
    if (!caseAEcrire) return
    caseAEcrire.valeur = valeur
    caseActive.value = null //permettra de fermer la bulle
}
function mettreCouleur(index:number): string{ //on donne une classe pour les chiffres
    const CaseAcolorier = grilleDeJeu.value[index]
    if (!CaseAcolorier){return ''}
    if (CaseAcolorier.verrouille === true){
        return 'text-black'  //renvoie une classe pour tailwindcss t donc va le colorier avec celle-ci
    }
    else if (CaseAcolorier.valeur === null){return ''}
    else if (!peutPlacer(grilleDeJeu.value,index,CaseAcolorier.valeur) || !testerInequation(grilleDeJeu.value,ineqDeJeu.value,index)){
        return 'text-red-400'
    } else {
        return 'text-green-500'
    }
} 

function respecterIneq(ineq:Inequation):boolean { //ineq.case est un index pas la valeur
    const val1 = grilleDeJeu.value[ineq.case1]?.valeur
    const val2 = grilleDeJeu.value[ineq.case2]?.valeur
    if (val1 == null || val2 == null) {return true} //si une valeur null alors pas tester
    if (ineq.signe === '<' && val1 > val2) {return false} //deux inéquations fausses
    if (ineq.signe === '>' && val1 < val2) {return false}
    return true
}

function colorierSigne(endroit: Endroit): string {
    const ineq = trouverInequation(endroit) //quelle ineq on vérifie ?
    if (ineq == null) {return 'text-black'} //si y'en a pas
    if (!respecterIneq(ineq)) {return 'text-red-400'} // si violé
    return 'text-black'//si y'a et rien violé
}

const tailleChoisie = ref(tailleGrille.value)

function nouveauJeu(){
    caseActive.value = null
    tailleGrille.value = tailleChoisie.value 
    const nouvelleGrille = GrilleVide(tailleGrille.value)
    placerChiffre(nouvelleGrille,0)
    const IneqCalcul = grilleInequation(nouvelleGrille)
    enleverEle(nouvelleGrille,IneqCalcul)
    grilleDeJeu.value = nouvelleGrille
    ineqDeJeu.value = IneqCalcul
}

//générer première grille à la fin pour avoir toutes les constantes au même endroit

const etatDeEcran = ref<'accueil' | 'jeu'>('accueil')
function lancerJeu(){
    if (etatDeEcran.value === 'accueil'){
        nouveauJeu()
        return etatDeEcran.value = 'jeu'
    } else {return etatDeEcran.value = 'accueil'}
}

</script>   

<template>
    <div class="flex items-center flex-col gap-6 py-8">
        <p class="text-center text-4xl font-semibold font-serif m-4"> Futoshiki </p>
        <div v-if="etatDeEcran === 'accueil'" class="flex flex-col items-center justify-center gap-3">
            <p class="mt-5 text-left"> <i><b>Règle :</b> <br/> - On ne peut pas avoir le même chiffre sur une même colonne ou une même ligne<br/> 
            - Pour résoudre le puzzle il s'aider des inéquations</i></p>
            <div class="flex items-center justify-center gap-4 mt-10">
                <button @click="lancerJeu" class="bg-rose-300 text-black text-6xl font-semibold active:scale-95 px-6 py-4 rounded-2xl transition-all hover:bg-rose-400">Jouer !</button>
                <select v-model="tailleChoisie" class="bg-rose-200 text-3xl hover:bg-rose-300">
                    <option v-for="n in 4" :key="n" :value="n + 2">{{ n+2 }}</option>
                </select>
            </div>
        </div>
        <div v-else>
        <div :style="{ display:'grid', gridTemplateColumns: pistes, gridTemplateRows: pistes, position : 'relative'}"> <!--déclare les pistes une par un !! '=' veut dire la valeur dont est contenu qlch, ':' signifie que suite est du js -->
            <template v-for="(endroit, k) in endroits" :key="k"> <!--répéte contenu sans reproduire d'élé HTML !!!! K est index de endroits-->
                <div v-if="endroit.forme === 'carre'" :style="{border: '2px solid #334155', position: 'relative'}" class="flex items-center justify-center"  @click="rendreActif(endroit.index)" :class="{'bg-rose-200': endroit.index === caseActive }"> <!--les : devant class va d'épendre d'une condition pour s'afficher-->
                    <div v-if="grilleDeJeu[endroit.index]?.valeur !== null" :class="mettreCouleur(endroit.index)" class="text-2xl font-semibold ">{{ grilleDeJeu[endroit.index]?.valeur }}</div>
                    <div v-if="endroit.index === caseActive" class="text-xl top-full left-0 z-10 flex gap-2 rounded-2xl bg-rose-300  " style="position: absolute">
                        <button v-for="chiffre in ListeTailGril" :key="chiffre"  @click.stop="ecrireValeur(chiffre)" class="rounded-3xl hover:bg-rose-400 transition-colors font-semibold px-2 py-2">{{chiffre}}</button> <!--click.stop permet d'arreter la porpagation du click-->
                        <button  @click.stop="ecrireValeur(null)" class=" rounded-3xl hover:bg-rose-400 transition-colors font-semibold px-2 py-2">X</button>
                    </div>
                </div> <!--là ici on crée les carrés avec les bords-->
                <div v-else-if="endroit.forme === 'horizontal' || endroit.forme === 'vertical'" class="min-h-full min-w-full flex items-center justify-center overflow-hidden"> <!--l'emplacement vide permet de décaler grille ça veut dire d'avoir que un carrée sur deux, donc inéquation ici-->
                    <div v-if="trouverSigne(endroit)" :style="{ transform: endroit.forme === 'vertical' ? 'rotate(90deg)' : 'rotate(0deg)'}" :class="colorierSigne(endroit)" class="text-2xl font-semibold leading-none inline-block">  {{ trouverSigne(endroit) }}
                    </div>
                </div> 
                <div v-else-if ="endroit.forme === 'espace'"></div>
            </template> <!--si on note template/ c'est une balise qui s'autoferme-->
            <div v-if="grilleFinie" class="absolute inset-0 flex items-center justify-center bg-white/75">
                <div class="px-5 py-3 rounded-2xl bg-blue-300 text-zinc-700 text-center shadow-2xl">
                    <p class="font-bold text-3xl">Félicitations !<br /> Tu as résolu la grille !</p>
                    <p class="text-1xl mt-2"><i>Teste une nouvelle taille!</i></p>
                </div>
            </div> <!--inset-0 se colle au 4 bord du parent--> 
        </div> <!--On crée un nouveau parent pas de la grille pour créer un bouton en dessus de celle-ci-->
        <div  class="flex items-center justify-center gap-3 mt-3">
            <button v-if="!grilleFinie" @click="nouveauJeu" class="bg-blue-300 text-zinc-700 font-semibold active:scale-95 px-4 py-2 rounded-2xl transition-all hover:bg-rose-300 mt-3">Nouvelle grille</button>
            <button v-if="!grilleFinie" @click="lancerJeu" class="bg-blue-300 text-zinc-700 font-semibold active:scale-95 px-4 py-2 rounded-2xl transition-all hover:bg-rose-300 mt-3">Accueil</button>
            <button v-if="grilleFinie" @click="nouveauJeu" class="bg-blue-300 text-zinc-700 font-semibold active:scale-95 px-4 py-2 rounded-2xl transition-all hover:bg-rose-300 mt-3 ">Nouvelle Partie !</button>      
            <select v-if="grilleFinie" v-model="tailleChoisie" class="bg-blue-200 hover:bg-blue-300 text-xl mt-3">
                <option v-for="n in 4" :key="n" :value="n + 2">{{ n+2 }}</option>
            </select> 
        </div>
        </div>
    </div>
</template>