<script lang="ts">
import { ref } from 'vue'

export default {
  data() {
    return {
      searchName: '',
      searchSize: '',
      searchLocation: '',
      items: [
        { id: 1, name: 'T-Shirt', size: 'M', location: 'Lager A' },
        { id: 2, name: 'Jeans', size: 'L', location: 'Lager B' },
        { id: 3, name: 'Socken', size: 'S', location: 'Lager C' },
        // weitere Produkte hier hinzufügen...
      ],
      showButtons: false // Eine Variable, um zu verfolgen, ob die Buttons angezeigt werden sollen
    };
  },
  computed: {
    filteredItems() {
      return this.items.filter(item => {
        return (
            item.name.toLowerCase().includes(this.searchName.toLowerCase()) &&
            item.size.toLowerCase().includes(this.searchSize.toLowerCase()) &&
            item.location.toLowerCase().includes(this.searchLocation.toLowerCase())
        );
      });
    }
  },
  methods: {
    // Eine Methode, um zu prüfen, ob die Eingabefelder leer sind und die Buttons entsprechend anzuzeigen
    checkInput() {
      if (this.searchName || this.searchSize || this.searchLocation) {
        this.showButtons = true;
      } else {
        this.showButtons = false;
      }
    },
    clearInputs() {
      this.searchName = ''; // Leeren Sie den Namen
      this.searchSize = ''; // Leeren Sie die Größe
      this.searchLocation = ''; // Leeren Sie den Lagerort
    },
    searchItems() {
      // Eine einfache Implementierung der Suchlogik, die die Suchkriterien in der Konsole ausgibt
      console.log('Search Items with criteria:');
      console.log('Name:', this.searchName);
      console.log('Size:', this.searchSize);
      console.log('Location:', this.searchLocation);
    }


  }


};
</script>

<template>
  <div>
    <h2>Textil Lagerlogistik</h2>
    <input type="text" v-model="searchName" placeholder="Suche nach Name" @input="checkInput">
    <input type="text" v-model="searchSize" placeholder="Suche nach Größe" @input="checkInput">
    <input type="text" v-model="searchLocation" placeholder="Suche nach Lager" @input="checkInput">

    <!-- Buttons nur anzeigen, wenn Eingaben gemacht wurden -->
    <div v-if="showButtons">
      <button @click="searchItems">Search</button>
      <button @click="clearInputs">Clear Inputs</button>
    </div>

    <ul>
      <li v-for="item in filteredItems" :key="item.id">
        {{ item.name }} - Größe: {{ item.size }} - Lager: {{ item.location }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
/* Styles hier anpassen */
</style>
