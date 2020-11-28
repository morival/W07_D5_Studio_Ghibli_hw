<template>
  <div id="selected-film" v-if="film">
      <button v-if="!film.isFavourite" v-on:click="addFavourite">
          Add to Favourites
      </button>
      <button v-if="film.isFavourite" v-on:click="removeFavourite">
          Remove from Favourites
      </button>
      <h2> {{film.title}} ({{film.release_date}})</h2>
      <h3>Director: {{film.director}} </h3>
      <h3>Producer: {{film.producer}} </h3>
      <p> {{film.description}} </p>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'film-detail',
    props: ['film'],
    methods: {
        addFavourite: function(){
            eventBus.$emit('favourite-added', this.film);
        },
        removeFavourite: function(){
            eventBus.$emit('favourite-removed', this.film);
        }
    }
}
</script>

<style>

</style>