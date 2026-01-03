<template>
  <div id="app">
    <MenuMemory v-if="!niveauChoisi" @startGame="startGame" />
    <GameBoard v-else :level="niveauChoisi" @finDePartie="finDePartie" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import MenuMemory from './components/Menu.vue'
import GameBoard from './components/GameBoard.vue'

const niveauChoisi = ref(null)  // null = menu affiché

// Lorsque le joueur clique sur "Démarrer"
function startGame(level) {
  niveauChoisi.value = level
}

// Quand la partie se termine, tu peux remettre le menu si tu veux
function finDePartie(score) {
  alert("Partie terminée ! Score : " + score)
  niveauChoisi.value = null // revenir au menu
}
</script>
