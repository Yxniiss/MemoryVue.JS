<template>
  <div class="card" @click="$emit('retournerCard')"> <!--émission de l'événement au clic on l'envoie au parent GameBoard.vue-->
    <div class="card-inner" :class="{ retournerCard: retourner }"> <!--ajout de la classe-->
       <div class="card-back" >?</div> <!--contenu de la face arrière donc ? par defaut-->
      <div class="card-front" >{{ value }}</div> <!--contenu de la face avant value = number regarde le props-->
    </div>
  </div>
</template>


<script setup>

    const { value, retourner } = defineProps({  //props reçus du parent
        value: Number, // valeur de la carte
        retourner: Boolean // état de la carte (retournée ou non)
    })
    
</script> 

<style>
    .card-back {
  background-color: blue;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 2rem;
}

.card-front {
  background-color: white;
  color:black;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
}

/* optionnel : cacher la face avant quand pas retournée */
.card-inner {
  position: relative;
}
.card-inner .card-front {
  display: none;
}
.card-inner.retournerCard .card-front {
  display: flex;
}
.card-inner.retournerCard .card-back {
  display: none;
}

</style>