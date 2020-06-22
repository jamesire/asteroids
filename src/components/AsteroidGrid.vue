<template>
<div class="float-right">
    <div class="card"> 
        <h2 class="card-header">{{header}}</h2> 
        <div class="m-2"> 
            <p>From: <Datepicker :value="fromDate" name="fromDate" /></p>
            To: <Datepicker :value="toDate" name="toDate" />
        </div> 
        <table class="table" :class="[{'table-dark': false}, 'table-bordered']"> 
            <thead class="thead-light"> 
                <th>Name</th> 
                <!-- <th>Close Approach Date</th> 
                <th>Miss Distance</th>  -->
            </thead> 
            <tbody is="transition-group" name="neo-list" v-cloak> 
                <tr v-for="(a) in asteroids" 
                    :key="a.neo_reference_id" 
                    :class="{highlight: isMissingData(a), 'shadow-sm': true}"> 
                    <td>{{a.name}}</td> 
                    <!-- <td>{{getCloseApproachDate(a)}}</td> 
                    <td> 
                        <ul v-if="a.close_approach_data.length > 0"> 
                            <li v-for="(value, key) in a.close_approach_data[0].miss_distance" :key="key"> 
                                {{key}}: {{value}} 
                            </li> 
                        </ul> 
                    </td>  -->
                </tr> 
            </tbody> 
        </table> 
    </div>
    </div>
</template>

<script>
    import Datepicker from 'vuejs-datepicker'

    export default {
        props: ['asteroids', 'header'],
        data: function() {
            return {            
                showSummary: true,
                fromDate: new Date(),
                toDate: null
            }
        },  
        components: {
            Datepicker
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
            }
        }
    }
</script>

<style scoped>
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
</style>