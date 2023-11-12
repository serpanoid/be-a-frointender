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
    margin-bottom: 20px;
    padding: 15px;
    background-color: #f9f9f9;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .pilot h3 {
    color: #0077cc;
    margin-bottom: 10px;
  }
  
  .pilot p {
    margin: 5px 0;
    color: #333;
    font-size: 14px;
  }
  
  .error {
    color: red;
    margin: 20px 0;
    text-align: center;
  }
  </style>
  
  