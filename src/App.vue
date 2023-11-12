<template>
  <div>
    <h1>Star Wars Starships</h1>
    <div v-if="loading">Loading starships...</div>
    <div v-if="error" class="error">Error: {{ error }}</div>
    <div v-if="starships" class="starship-list">
      <div v-for="ship in starships" :key="ship.name" class="starship">
        <h2>{{ ship.name }}</h2>
        <div class="starship-details">
          <p>Model: {{ ship.model }}</p>
          <p>Manufacturer: {{ ship.manufacturer }}</p>
          <p>Cost in Credits: {{ ship.cost_in_credits }}</p>
          <p>Length: {{ ship.length }}</p>
          <p>Max Atmosphering Speed: {{ ship.max_atmosphering_speed }}</p>
          <p>Crew: {{ ship.crew }}</p>
          <p>Passengers: {{ ship.passengers }}</p>
          <p>Cargo Capacity: {{ ship.cargo_capacity }}</p>
          <p>Consumables: {{ ship.consumables }}</p>
          <p>Hyperdrive Rating: {{ ship.hyperdrive_rating }}</p>
          <p>MGLT: {{ ship.MGLT }}</p>
          <p>Starship Class: {{ ship.starship_class }}</p>
          <button v-if="ship.pilots.length" @click="showPilots(ship)" class="view-pilots-button">View Pilots</button>
        </div>
        <PilotList v-if="activeShip === ship" :pilotUrls="ship.pilots" />
      </div>
    </div>
    <div class="pagination">
      <button @click="loadPage(prevPageUrl)" :disabled="!prevPageUrl">Previous</button>
      <button @click="loadPage(nextPageUrl)" :disabled="!nextPageUrl">Next</button>
    </div>
  </div>
</template>

<script>
import PilotList from './components/PilotList.vue';
import axios from 'axios';

export default {
  components: {
    PilotList
  },
  data() {
    return {
      starships: null,
      loading: false,
      error: null,
      currentPage: 1,
      nextPageUrl: null,
      prevPageUrl: null,
      activeShip: null
    };
  },
  created() {
    this.fetchStarships();
  },
  methods: {
    fetchStarships(url = 'https://swapi.dev/api/starships') {
      this.loading = true;
      axios.get(url)
        .then(response => {
          this.starships = response.data.results;
          this.nextPageUrl = response.data.next;
          this.prevPageUrl = response.data.previous;
        })
        .catch(error => {
          console.error(error);
          this.error = 'Failed to load starships';
        })
        .finally(() => {
          this.loading = false;
        });
    },
    loadPage(url) {
      if (url) {
        this.fetchStarships(url);
      }
    },
    showPilots(ship) {
    this.activeShip = this.activeShip === ship ? null : ship;
    }
  }
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f4;
  color: #333;
}

h1 {
  text-align: center;
  color: #333;
}

.starship {
  margin-bottom: 30px;
  padding: 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.starship h2 {
  color: #0077cc;
  margin-bottom: 10px;
}

.starship-details p {
  font-size: 16px;
  line-height: 1.6;
}

.button {
  display: inline-block;
  padding: 10px 20px;
  margin-right: 10px;
  border: none;
  border-radius: 5px;
  background-color: #0077cc;
  color: white;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: background-color 0.3s;
}

.button:hover {
  background-color: #005fa3;
  cursor: pointer;
}

.button:disabled {
  background-color: #cccccc;
  cursor: default;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 30px;
}

.pagination button {
  padding: 8px 15px;
  margin: 5px;
  background-color: #008CBA;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.pagination button:hover {
  background-color: #0077a3;
}

.pagination button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.error {
  color: red;
  text-align: center;
}

.view-pilots-button {
  padding: 8px 15px;
  margin-top: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.view-pilots-button:hover {
  background-color: #45a049;
}

</style>
