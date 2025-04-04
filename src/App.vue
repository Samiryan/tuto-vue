<template>
  <div class="container">
    <h2>Ma To-Do List</h2>
    <div class="input-group">
      <input v-model="nouvelleTache" placeholder="Ajouter une tâche" />
      <button @click="ajouterTache">Ajouter</button>
    </div>
    <ul>
      <li v-for="(tache, index) in taches" :key="index" :class="{ termine: tache.termine }">
        <span @click="toggleTache(index)">{{ tache.nom }}</span>
        <button @click="supprimerTache(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const nouvelleTache = ref('');
    const taches = ref([]);

    function ajouterTache() {
      if (nouvelleTache.value.trim() !== '') {
        taches.value.push({ nom: nouvelleTache.value, termine: false });
        nouvelleTache.value = '';
      }
    }

    function supprimerTache(index) {
      taches.value.splice(index, 1);
    }

    function toggleTache(index) {
      taches.value[index].termine = !taches.value[index].termine;
    }

    return { nouvelleTache, taches, ajouterTache, supprimerTache, toggleTache };
  }
};
</script>

<style>
.container {
  max-width: 400px;
  margin: auto;
  text-align: center;
}
.input-group {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}
input {
  flex: 1;
  padding: 5px;
}
button {
  padding: 5px 10px;
  cursor: pointer;
}
li {
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px;
  border-bottom: 1px solid #ddd;
}
.termine {
  text-decoration: line-through;
  color: gray;
}
</style>
