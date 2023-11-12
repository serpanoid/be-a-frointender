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
          <button v-if="ship.pilots.length" @click="showPilots(ship)">View Pilots</button>
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
.starship {
  margin-bottom: 20px;
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
}

.starship h2 {
  margin: 0;
  font-size: 24px;
}

.starship-details p {
  margin: 5px 0;
  font-size: 14px;
}

.error {
  color: red;
}

button {
    margin: 5px;
    padding: 5px 10px;
  }

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>
