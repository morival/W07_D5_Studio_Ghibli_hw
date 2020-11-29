<template>
  <div id="selected-film" v-if="film">
      <h2> {{film.title}} ({{film.release_date}})</h2>
      <button v-if="!film.isFavourite" v-on:click="addFavourite">
          Add to Favourites
      </button>
      <button v-if="film.isFavourite" v-on:click="removeFavourite">
          Remove from Favourites
      </button>
      <h3>Director: {{film.director}} </h3>
      <h3>Producer: {{film.producer}} </h3>
      <p> {{film.description}} </p>
      <div id="species-list" v-if="species.length" :species="species">
        <h3>Species:</h3>
        <ul>
            <div v-for="(creature, index) in species" :creature="creature" :key="index">
                <li> {{creature.name}} </li>
            </div>
        </ul>
      </div>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'film-detail',
    props: ['film'],
    data(){
        return{
            species:[]
        }
    },
    mounted(){
        this.getSpecies();
    },
    watch: {
        film: function(){
            this.getSpecies()
        }
    },
    methods: {
        getSpecies(){
            const speciesPromises = this.film.species.map((person) => {
                return fetch(person).then(res => res.json());
            })
            Promise.all(speciesPromises)
            .then((data) => {
                this.species = data;
            })
        },
        addFavourite: function(){
            eventBus.$emit('favourite-added', this.film);
        },
        removeFavourite: function(){
            eventBus.$emit('favourite-removed', this.film);
        }
    },
}
</script>

<style>

</style>