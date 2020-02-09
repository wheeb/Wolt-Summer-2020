<template>
    <div id="app">
        <div>
            <p class="mainHeader" >Foodmaster </p>
            <p class="mainHeaderP">restaurant search</p>
            <p class="headerTitle">Site shows 50 restaurants from Helsinki area. Data is loaded from predetermined json dataset.</p>
        </div>
        <table>
            <thead>
            <p class="header1">Sort by name</p>
            <button @click="sort('name')" > Name: {{currentSortDir}}</button>
            </thead>
            <tbody>
            <div class="flex-container">
                <tr v-for="restaurant in sortedRestaurants" :key="restaurant.name" >
                    <div class="innerBox">
                        <div>
                            <td>
                                <img
                                        v-bind:src="restaurant.image">
                            </td>
                        </div>
                        <div class="textArea">
                            <td><p class="rowTitle">{{restaurant.name}}</p></td>
                            <br>
                            <div>
                                <div>
                                    <td>{{restaurant.description}}</td>
                                </div>
                                <div>
                                    <td>{{restaurant.city}}</td>
                                </div>
                            </div>
                        </div>
                    </div>
                </tr>
            </div>
            </tbody>
        </table>
    </div>
</template>

<script>
    import json from './assets/restaurants.json'
    export default {
        data(){
            return{
                restaurants: json,
                reverse: false,
                currentSort:'name',
                currentSortDir:'ascending'
            }
        }, /*end of data*/

        methods:{
            sort:function(s) {
                // when we sort by the same column, this just flips the direction.
                //if s == current sort, reverse
                if(s === this.currentSort) {
                    this.currentSortDir = this.currentSortDir==='ascending'?'descending':'ascending';
                }
                this.currentSort = s;
            }
        },
        computed:{
            sortedRestaurants:function() {
                // eslint-disable-next-line vue/no-side-effects-in-computed-properties
                return this.restaurants.restaurants.sort((a,b) => {
                    //modifier number handles reversing the numbers based on the direction of the sort
                    let modifier = 1;
                    if(this.currentSortDir === 'descending') modifier = -1;
                    if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
                    if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
                    return 0;
                });
            }
        }
    }
</script>

<!-- "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #app{
        font-family: 'Ubuntu', sans-serif;
    }
    th {
        cursor: pointer;
        padding: 5px;
    }
    .innerBox{
        padding-bottom: 10px;
        max-width: 350px;
        margin-left: 5%;
        margin-right: 5%;
        margin-bottom: 5%;
        border: 2px solid #6b6b6b;
        border-radius: 30px;
    }
    .textArea{
        padding-left: 5px;
    }
    .flex-container{
        display: flex;
        flex-wrap: wrap;
    }
    .rowTitle{
        align-content: center;
        font-size: 1.5em;
    }
    .header1{
        font-size: 2em;
    }
    .mainHeader{
        display: inline;
        font-weight: 900;
        font-size: 1.5em;
    }
    .mainHeaderP{
        display: inline;
    }
    img {
        width: 100%;
        border-radius: 30px;
    }
    h3 {
        margin: 40px 0 0;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
</style>
