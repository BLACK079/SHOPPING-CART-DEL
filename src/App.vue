<script setup>
import { ref, computed } from 'vue';
const header = ref('App Lista de compras');
const items = ref([
  { id: 1, label: '10 bolillos', purchased: true, highPriority: true},
  { id: 2, label: '1 lata de frijoles', purchased: false, highPriority: true},
  { id: 3, label: '2 lata de atún', purchased: true, highPriority: true}
]);
//Funcion que alterna el estado de compra de un item
const togglePurchased = (item) => {
  //Invertir la propiedad purchased
  item.purchased = !item.purchased;
}
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value })
  //Limpiando el contenido de newItem
  newItem.value = ""; 
};
const newItem = ref('');
const newItemHighPriority = ref(false);
const editing = ref(false);
const doEdit = (edit) => {
  //alteramos la variable esditing
  editing.value = edit;
  // limpiamos el input de texto
  newItem.value = ""; 
};
// Creando una propiedad computada
const characterCount = computed(()=>{
  // Toda propiedad computada debe regresar un valor
  return newItem.value.length;
});
// Creando propiedad computada que invierte items de la lista
const reversedItems = computed(() => {
  return [...items.value].reverse();
});
</script>

<template>
  <div class="header">
    <h1> <i class="material-icons shopping-cart-icon">local_mall</i> {{ header }}</h1>
    <button v-if="!editing" @click="doEdit(true)" class="btn btn-primary">Agregar</button>
    <button v-else @click="doEdit(false)" class="btn btn-cancel">Cancelar</button>
  </div>
  <!-- <a v-bind:href="newItem">
    <i class="material-icons shopping-cart-icon">link</i>
  </a> -->
  <form v-if="editing" v-on:submit.prevent= saveItem class="add-item form">
    <!-- Input de nuevo articulo -->
    <input class="addint" v-model.trim="newItem" type="text" placeholder="Ingresar articulo" required>
    <!-- Check Boxes -->
    <label>
      <input v-model="newItemHighPriority" type="checkbox"> Alta Prioridad
    </label>
    {{ newItemHighPriority ? "🔥" : "🧊" }}
    <!-- boton en la UI -->
    <button :disabled="newItem.length ===0" class="btn btn-primary">Salvar articulo</button>	
    <!-- Contador -->
  <p class="counter">
    {{ characterCount }} / 200
  </p>

  </form>
  <ul>
    <li v-for="({ id, label, purchased, highPriority }, index) in reversedItems"
     :class="{strikeout : purchased, priority : highPriority}"
     @click="togglePurchased(reversedItems[index])"
     v-bind:key="id">
      🔹 {{ label }}
      
    </li>
  </ul>
  <p v-if ="items.length === 0">🥀 Lista de Compras Vacia 🥀</p>
</template>

<style>
.shopping-cart-icon {
  font-size: 2rem;
}
</style>