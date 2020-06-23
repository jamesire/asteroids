<template>
  <div id="app">
      <Earth />
      <Asteroid ref="asteroid" />
      <AsteroidGrid :asteroids="asteroids" @toggleAccordian="toggleAccordian" @magnifyAsteroids="magnifyAsteroids" />
  </div>
</template>

<script>
import AsteroidGrid from './components/AsteroidGrid.vue'
import Earth from './components/Earth.vue'
import Asteroid from './components/Asteroid.vue'
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'app',
  components: {
    Earth,
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
          var startDate = moment().subtract(7, 'days').calendar();
          var endDate = new Date();

          startDate = moment(String(startDate)).format('YYYY-MM-DD')
          endDate = moment(String(endDate)).format('YYYY-MM-DD')

          var url = 'https://api.nasa.gov/neo/rest/v1/feed?start_date=' + startDate + '&end_date=' + endDate + '&api_key=' + process.env.VUE_APP_API_KEY;
          axios.get(url)
              .then(res => {                                       
                  var neos = Object.entries(res.data.near_earth_objects);
                  this.asteroids = [];

                  for(var data of neos) {
                      this.asteroids = [].concat(this.asteroids, data[1]);
                  }

                  this.asteroids.forEach(function (a) {
                    a.open = false;

                    a.estimated_diameter.kilometers.estimated_diameter_max = Number((a.estimated_diameter.kilometers.estimated_diameter_max).toFixed(3))
                    a.estimated_diameter.kilometers.estimated_diameter_min = Number((a.estimated_diameter.kilometers.estimated_diameter_min).toFixed(3))
                  })

                  this.asteroids.sort(function(a, b) {
                    var aMax = a.estimated_diameter.kilometers.estimated_diameter_max;
                    var aMin = a.estimated_diameter.kilometers.estimated_diameter_min;

                    var bMax = b.estimated_diameter.kilometers.estimated_diameter_max
                    var bMin = b.estimated_diameter.kilometers.estimated_diameter_min

                    if((bMax * bMin) > (aMax * aMin)) {
                      return 1;
                    }
                    else {
                      return -1;
                    }
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
            },
            magnifyAsteroids: function(mult) {
              this.$refs.asteroid.applyMagnification(mult);
            } 
    }
}
</script>

<style>
  [v-cloak] {
    display: none;
  }
</style>
