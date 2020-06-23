<template>
  <div id="app">
  <Asteroid ref="asteroid" />
    <AsteroidGrid :asteroids="asteroids" @toggleAccordian="toggleAccordian" header="Near-Earth Objects" />
  </div>
</template>

<script>
import AsteroidGrid from './components/AsteroidGrid.vue'
//import Earth from './components/Earth.vue'
import Asteroid from './components/Asteroid.vue'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    //Earth,
    Asteroid,
    AsteroidGrid
  },
  data() {
      return {
        asteroids: [],
        selectedAsteroid: null
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
                  this.asteroids = res.data.near_earth_objects;
                  this.asteroids.forEach(function (a) {
                    a.open = false;

                    a.estimated_diameter.kilometers.estimated_diameter_max = Number((a.estimated_diameter.kilometers.estimated_diameter_max).toFixed(3))
                    a.estimated_diameter.kilometers.estimated_diameter_min = Number((a.estimated_diameter.kilometers.estimated_diameter_min).toFixed(3))
                  })
              });
      },
      toggleAccordian: function (i) {
                this.asteroids = this.asteroids.map((asteroid, index) =>{
                    if(i === index) {
                        asteroid.open = !asteroid.open;
                        this.selectedAsteroid = asteroid;

                        var height = asteroid.estimated_diameter.kilometers.estimated_diameter_max;
                        var width = asteroid.estimated_diameter.kilometers.estimated_diameter_min;

                        this.$refs.asteroid.setDimensions(height, width);
                    } 
                    else {
                      asteroid.open = false;
                    }
                    return asteroid;
                })
                //this.showClass === "" ? this.showClass = "collapse show" : this.showClass = "";
            }  
    }
}
</script>

<style>
  [v-cloak] {
    display: none;
  }
</style>
