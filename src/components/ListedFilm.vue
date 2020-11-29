<template lang="html">
    <div class="listed-film">
        <p v-on:click="handleClick"> {{film.title}}</p>
        <button v-if="!film.isFavourite" v-on:click="addFavourite">Add to Favourites</button>
        <button v-if="film.isFavourite" v-on:click="removeFavourite">Remove from Favourites</button>
        
    </div>
  
</template>

<script>
import {eventBus} from '@/main.js';

export default {
    name: 'listed-film',
    props: ['film'],
    methods: {
        handleClick: function(){
            eventBus.$emit('film-selected', this.film);
        },
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