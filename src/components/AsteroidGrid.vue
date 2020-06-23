<template>
<div class="float-right">
    <div class="card"> 
        <h2 class="card-header">Near-Earth Objects</h2> 
        <div class="card-body">
            Magnify Asteroids:
            <div class="d-flex justify-content-center">
                <span class="font-weight-bold mr-2"><i class="fa fa-minus" aria-hidden="true"></i></span>
                <form class="range-field">
                    <input class="border-0" type="range" min="1" max="3" />
                </form>
                <span class="font-weight-bold ml-2"><i class="fa fa-plus" aria-hidden="true"></i></span>
            </div>
        </div>
        <div class="asteroid-container" v-cloak>
            <span class="asteroid-iterator" v-for="(asteroid, i) in asteroids" :key="i">
                <div :class="asteroid.open ? 'asteroid-list open' : 'asteroid-list'"  @click="$emit('toggleAccordian', i)">
                    <div class="asteroid-name">{{asteroid.name}}</div>
                    <div class="asteroid-detail">
                        <b>Diameter: </b>{{asteroid.estimated_diameter.kilometers.estimated_diameter_max}}km x {{asteroid.estimated_diameter.kilometers.estimated_diameter_min}}km
                            <br />
                        <b>Approach Date: </b>{{getCloseApproachDate(asteroid)}}
                            <!-- <br />
                        <b>Distance: </b>{{getDistance(asteroid)}}km -->
                    </div>
                </div>
            </span>
        </div>
    </div>  
    </div>
</template>

<script>
    export default {
        props: ['asteroids', 'header'],
        data: function() {
            return {            
                showSummary: true,
                fromDate: new Date(),
                toDate: null,
                selectedAstroid: null,
            }
        },  
        components: {
        },
        computed: {
            numAsteroids: function() {
                return this.asteroids.length;
            },
            closestObject: function() {
                var neosHavingData = this.asteroids.filter(function(neo) {
                    return neo.close_approach_data.length > 0;
                });                
                var simpleNeos = neosHavingData.map(function(neo) {
                    return {name: neo.name, miles: neo.close_approach_data[0].miss_distance.miles};        
                });
                var sortedNeos = simpleNeos.sort(function(a, b) {
                    return a.miles - b.miles;        
                });
                return sortedNeos[0].name;
            }
        },  
        methods: {
            getCloseApproachDate: function (a) {
                if (a.close_approach_data.length > 0) {
                    return a.close_approach_data[0].close_approach_date;   
                }
                return 'N/A';
            },
            getRowStyle: function (a) {
                if (a.close_approach_data.length == 0) {
                    return {
                        border: 'solid 2px red',
                        color: 'red'
                    }
                }
            },
            isMissingData: function (a) {
                return a.close_approach_data.length == 0;                    
            },
            getDistance: function(asteroid) {
                if(asteroid.close_approach_data) {
                    var distanceInKm = asteroid.close_approach_data[0].miss_distance.kilometers;
                    var roundedDistance = Number((distanceInKm).toFixed(3));

                    return roundedDistance;
                }
                return "N/A";
            }
        }
    }
</script>

<style scoped>
    [v-cloak] {
        display: none;
    }
    .highlight {
        border: solid 3px red;
        color: red;
    }
    .shooting-star-leave-to, .shooting-star-enter {
        transform: translateX(150px) rotate(30deg);
        opacity: 0;
    }
    .shooting-star-enter-active, .shooting-star-leave-active {
        transition: all .5s ease;
    }
    .neo-list-leave-to, .neo-list-enter {
        opacity: 0;
        transform: translateY(30px);
    }
    .neo-list-enter-active, .neo-list-leave-active {
        transition: all 1s linear;
    }

    .datepicker-line {
        float: left;
        margin-top: 10px;
    }

    .asteroid-container {
        height: 300px;
        max-height: 100%;
        overflow-y: auto;
    }

    .asteroid-name {
        padding-bottom: 6px;
        border-bottom: 1px solid rgba(0, 0, 0, .1);
    }

    .asteroid-list {
        display: block;
        width: 100%;
        padding: 10px;
        background-color: white;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.2);
        margin: 5px auto;
        border-radius: 8px;
        max-width: 700px;
    }

    .asteroid-list .asteroid-name{
        position: relative;
        color: #3c3c3c;
        font-size: 16px;
        transition: all 0.2s linear;
    }

    .asteroid-list .asteroid-name::after {
        content: "";
        position: absolute;
        top: 50%;
        right: 0px;
        transform: translateY(-50%) rotate(0deg);
        width: 30px;
        height: 30px;
        background-image: url('../resources/caret.svg');
        background-position: center;
        background-size: 12px;
        transition: all 0.2s linear;
        background-repeat: no-repeat;
    }

    .asteroid-list.open .asteroid-name {
        margin-bottom: 15px;
    }

    .asteroid-list.open .asteroid-name::after {
        transform: translateY(-50%) rotate(90deg);
    }

    .asteroid-detail {
        color: #3c3c3c;
        font-size: 14px;
        opacity: 1;
        max-height: 0px;
        overflow-y: hidden;
        transition: all 0.2s ease-out;
    }

    .asteroid-list.open .asteroid-detail {
        opacity: 1;
        max-height: 1000px;
    }
</style>