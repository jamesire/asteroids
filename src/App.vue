<template>
  <div id="app">
    <Earth />
    <AsteroidGrid :asteroids="asteroids" header="Near-Earth Objects" />
  </div>
</template>

<script>
import AsteroidGrid from './components/AsteroidGrid.vue'
import Earth from './components/Earth.vue'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    Earth,
    AsteroidGrid
  },
  data() {
      return {
        asteroids: []
      }
  },            
  created: function () {
      this.fetchAsteroids();
  },
  methods: {
      fetchAsteroids: function () {
          var url = 'https://api.nasa.gov/neo/rest/v1/neo/browse?api_key=' + process.env.VUE_APP_API_KEY;
          axios.get(url)
              .then(res => {                                       
                  this.asteroids = res.data.near_earth_objects.slice(0, 10);
              });
      },
  }
}
</script>

<style>
  [v-cloak] {
    display: none;
  }
</style>
