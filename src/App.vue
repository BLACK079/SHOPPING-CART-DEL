<script setup>
import { ref } from 'vue';
const header = ref('App Lista de compras');
const items = ref([
  // { id: 1, label: '10 bolillos' },
  // { id: 2, label: '1 lata de frijoles' },
  // { id: 3, label: '2 lata de atún' }
]);
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
    {{ newItemHighPriority ? "👍" : "👈" }}
    <!-- boton en la UI -->
    <button class="btn btn-primary">Salvar articulo</button>
  </form>
  <ul>
    <li v-for="{ id, label } in items" v-bind:key="id">
      🔹 {{ label }}
    </li>
  </ul>
  <p v-if ="items.length === 0">🥀 Lista de Compras Vacia 🥀</p>
  <p v-else>😎 Ingrese más Items</p>
</template>

<style>
.shopping-cart-icon {
  font-size: 2rem;
}
</style>