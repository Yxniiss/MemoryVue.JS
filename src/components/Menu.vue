<template>
  <div class="menu">
    <h2>Menu Memory</h2> <!-- Titre du menu -->
    <label> 
      Choisir la taille de la grille :
      <select v-model="level"> <!-- liaison du select avec level  -->
        <option value="2">2x2</option> <!-- options pour le niveau de difficulté -->
        <option value="4">4x4</option> <!-- options pour le niveau de difficulté -->
        <option value="6">6x6</option> <!-- options pour le niveau de difficulté -->
      </select>
    </label>
    <button @click="startGame">Démarrer la partie</button> <!-- bouton pour démarrer la partie -->
    <button @click="lastGame">Dernière partie</button> <!-- bouton pour afficher la dernière partie sa je l'ai fait mais en v2v tu peux le supprimer -->
  </div>
</template>

<script setup>
import { ref } from 'vue'

const level = ref(4)  // valeur par défaut 4x4 si il choisie rien se sera ce niveau qui ressort 

// Fonction déclenchée quand on clique sur "Démarrer" --> le miens marcher pas ducoup chat gpt m'a corriger tout ce qui a en bas okaou
function startGame() {
  // On émet l'événement vers le parent (App.vue)
  // pour lancer le GameBoard avec le niveau choisi
  emit('startGame', Number(level.value))
}

// Fonction pour le bouton "Dernière partie"
function lastGame() {
  console.log("Dernière partie cliquée !")
}

// Déclarer l'émission d'événements
const emit = defineEmits(['startGame'])
</script>


<!--calcule pas le css chat gpt j'ai fait un truc en tac tac pour voir sa ressembler a quoi -->
<style>
.menu { 
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  font-family: Arial, sans-serif;
}
select, button {
  padding: 5px 10px;
  font-size: 1rem;
}
button {
  cursor: pointer;
}
</style>
