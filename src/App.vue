<template>
  <div id="app">
    <h1>Studio Ghibli</h1>
    <div id="list-info" v-if="films.length">
      <label for="film_select">Select a Film:</label>
      <select id="film_select" v-model="selectedFilm">
        <option disabled value="">Select a film</option>
        <option v-for="film in films" :key="film.id" :value="film"> {{film.title}} </option>
    </select>
    </div>

    <div class="main-container">
      <film-detail :film="selectedFilm"></film-detail>
    </div>
    
  </div>
</template>

<script>

import {eventBus} from './main.js';
import FilmDetail from './components/FilmDetail.vue'

export default {
  name: 'App',
  data(){
    return {
      films:[],
      selectedFilm: null,
      favouriteFilms: []
    }
  },
  components: {
    'film-detail': FilmDetail,
  },
  methods:{
    getFilms: function() {
      fetch('https://ghibliapi.herokuapp.com/films')
      .then(res => res.json())
      .then(films => this.films = films)
    }
  },
  mounted(){
    this.getFilms();
    eventBus.$on('film-selected', film => (this.selectedFilm = film));
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
