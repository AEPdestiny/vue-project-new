<script setup lang="ts">
import { onMounted, ref, computed, type Ref } from 'vue';

interface Kleidung {
  kleidungName: string;
  groesse: string;
  anzahl: number;
  lager: number;
}



const kleidungen: Ref<Kleidung[]> = ref([])
const baseUrl = import.meta.env.VITE_APP_BACKEND_BASE_URL
const endpoint = baseUrl+'/api/suche'

const searchName = ref('')
const searchGroesse = ref('')
const searchLager = ref('')


function requestKleidungen(): void {
  fetch(endpoint)
      .then((response) => response.json())
      .then((data) => {
        kleidungen.value = data;
      })
      .catch((error) => console.log(error));
}

const filteredKleidungen = computed(() => {
  return kleidungen.value.filter(kleidung => {
    const matchesName = kleidung.kleidungName.toLowerCase().includes(searchName.value.toLowerCase());
    const matchesGroesse = kleidung.groesse.toLowerCase().includes(searchGroesse.value.toLowerCase());
    const matchesLager = kleidung.lager.toString().includes(searchLager.value);
    return matchesName && matchesGroesse && matchesLager;
  });
});

onMounted(() => requestKleidungen());
</script>

<template>
  <div>
    <h1 class="orange-title">Suche</h1>
    <h2>Kleidung Liste</h2>
    <div class="search-container">
      <input v-model="searchName" type="text" placeholder="Suche nach Name" class="search-input" />
      <input v-model="searchGroesse" type="text" placeholder="Suche nach Größe" class="search-input" />
      <input v-model="searchLager" type="text" placeholder="Suche nach Lager" class="search-input" />
    </div>
    <table>
      <thead>
      <tr>
        <th>Name</th>
        <th>Größe</th>
        <th>Anzahl</th>
        <th>Lager</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="kleidung in filteredKleidungen" :key="kleidung.kleidungName">
        <td>{{ kleidung.kleidungName }}</td>
        <td>{{ kleidung.groesse }}</td>
        <td>{{ kleidung.anzahl }}</td>
        <td>{{ kleidung.lager }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.orange-title {
  color: #f97316; /* Orange color same as "Geschafft!" */
  font-size: 2em; /* One size larger than h2 */
  font-weight: bold; /* Make the text bold */
  margin-bottom: 0.5em;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background-color: #f2f2f2;
  text-align: left;
}

.search-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
}

.search-input {
  width: 30%;
  padding: 8px;
  box-sizing: border-box;
}
</style>
