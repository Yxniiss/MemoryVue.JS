<template>
    <h2>Difficulté : {{ level }}</h2> <!-- affichage du niveau reçu en props -->
    <h3>Score : {{ score }}</h3> <!-- affichage du score -->
    <Card v-for="(item, index) in table" :key="index" :value="item.value" :retourner="item.retournerCard" @retournerCard="handleCardClick(index)" /> <!-- appel de la fonction handleCardClick avec l'index de la carte -->

</template>

<script setup>
    import Card from './Card.vue' // import du composant Card.vue
    import { ref } from 'vue'
    const props = defineProps({  // réception des props
        level: Number   // niveau de difficulté (2, 4, 6)
    })

    function shuffle(array) {  // fonction de mélange des cartes
        for (let i = array.length - 1; i > 0; i--) { // boucle de la fin au début
            const j = Math.floor(Math.random() * (i + 1)); // index aléatoire
            [array[i], array[j]] = [array[j], array[i]]; // échange des éléments
        }
    }

    const table = ref([])  // tableau des cartes

    function initTable() { // initialisation du tableau des cartes
        const nombreCarte = props.level * props.level   // calcul du nombre de cartes
        const nombrePaires = nombreCarte / 2  // calcul du nombre de paires

        const tableTemp = [] // tableau temporaire pour stocker les cartes avant mélange , complexe donc on passe par une variable temporaire
        for (let i = 1; i <= nombrePaires; i++) { // boucle pour créer les paires
            tableTemp.push({ value: i, retournerCard: false, found: false }) // ajout de la première carte de la paire
            tableTemp.push({ value: i, retournerCard: false, found: false }) // ajout de la deuxième carte de la paire
        }
        table.value = tableTemp // affectation du tableau temporaire au tableau réactif table
        shuffle(table.value) // mélange des cartes
    }

    // Appeler initTable dès que le composant est monté
    initTable()


    let premiereCarte = null // pour stocker l'index de la première carte retournée
    let deuxiemeCarte = null // pour stocker l'index de la deuxième carte retournée
    let bloquer = false  // Pour bloquer les clics pendant le délai
    let score = ref(0)  // compteur d'score


    function handleCardClick(index) {
        if (bloquer) return; // ignore le clic si on attend

        // ignorer si la carte est déjà retournée ou trouvée
        if (table.value[index].retournerCard || table.value[index].found) return;

        // retourner la carte cliquée
        table.value[index].retournerCard = true;

        if (premiereCarte === null) { // première carte cliquée
            premiereCarte = index; // on stocke son index
        } else { // deuxième carte cliquée  
            deuxiemeCarte = index; // on stocke son index

            // on a maintenant deux cartes, on compare
            if (table.value[premiereCarte].value === table.value[deuxiemeCarte].value) { 
                // paire trouvée
                score.value += 20; // on ajoute des points
                table.value[premiereCarte].found = true; // on marque la première carte comme trouvée
                table.value[deuxiemeCarte].found = true; // on marque la deuxième carte comme trouvée
                premiereCarte = null; // réinitialisation pour la prochaine paire
                deuxiemeCarte = null; // réinitialisation pour la prochaine paire
            } else { // pas la même valeur → retour après 1 seconde
                bloquer = true; // on bloque les clics
                setTimeout(() => { // délai d'1 seconde
                    score.value -= 5; // on enlève des points
                    table.value[premiereCarte].retournerCard = false; // on retourne la première carte
                    table.value[deuxiemeCarte].retournerCard = false; // on retourne la deuxième carte
                    premiereCarte = null; // réinitialisation la première carte tire pour la prochaine paire
                    deuxiemeCarte = null; // réinitialisation la deuxième carte tire pour la prochaine paire
                    bloquer = false; // on débloque les clics
                }, 1000); 
            }

            const toutesTrouvees = table.value.every(card => card.found); // vérification si toutes les cartes sont trouvées
            if (toutesTrouvees) { // si toutes les cartes sont trouvées
                alert("Partie terminée ! Score : " + score.value) // affichage du score
}
        }
    }



</script>

<style>
    
</style>