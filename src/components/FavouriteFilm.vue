<template>
  <li>
      {{film.title}}
      <button v-on:click="selectFilm">Info</button>
      <button v-on:click="removeFavourite">Remove from Favourites</button>
  </li>
</template>

<script>
import {eventBus} from '@/main.js';

export default {
    props: ['film'],
    methods: {
        selectFilm: function(){
            eventBus.$emit('film-selected', this.film);
        },
        removeFavourite: function(){
            eventBus.$emit('favourite-removed', this.film);
        }
    }
}
</script>

<style>

</style>