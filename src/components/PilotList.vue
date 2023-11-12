<template>
    <div>
      <h2>Pilots</h2>
      <div v-if="loading">Loading pilots...</div>
      <div v-if="error" class="error">{{ error }}</div>
      <ul v-if="pilots.length">
        <li v-for="pilot in pilots" :key="pilot.name" class="pilot">
          <h3>{{ pilot.name }}</h3>
          <p>Height: {{ pilot.height }} cm</p>
          <p>Mass: {{ pilot.mass }} kg</p>
          <p>Hair Color: {{ pilot.hair_color }}</p>
          <p>Skin Color: {{ pilot.skin_color }}</p>
          <p>Eye Color: {{ pilot.eye_color }}</p>
          <p>Birth Year: {{ pilot.birth_year }}</p>
          <p>Gender: {{ pilot.gender }}</p>
        </li>
      </ul>
      <div v-else>No pilots available</div>
    </div>
  </template>
  <script>
  import axios from 'axios';
  
  export default {
    props: {
      pilotUrls: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        pilots: [],
        loading: false,
        error: null
      };
    },
    watch: {
      pilotUrls: {
        immediate: true,
        handler(urls) {
          this.fetchPilots(urls);
        }
      }
    },
    methods: {
      async fetchPilots(urls) {
        this.loading = true;
        this.error = null;
        try {
          const responses = await Promise.all(urls.map(url => axios.get(url)));
          this.pilots = responses.map(response => response.data);
        } catch (error) {
          console.error(error);
          this.error = 'Failed to load pilots';
        } finally {
          this.loading = false;
        }
      }
    }
  };
  </script>
  <style>
  .pilot {
    margin-bottom: 15px;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  .error {
    color: red;
  }
  </style>
  