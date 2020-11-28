<template>
  <div id="app">
    <h1>Studio Ghibli</h1>
    <div id="list-dropdown" v-if="films.length">
      <label for="film_select">Select a Film:</label>
      <select id="film_select" v-model="selectedFilm">
        <option disabled value="">Select a film</option>
        <option v-for="film in films" :key="film.id" :value="film"> {{film.title}} </option>
      </select>
    </div>
    <!-- <div id="list-info" v-if="films.length">
      <film-list :films="films"></film-list>
    </div> -->

    <div class="main-container">
      <film-detail v-if="selectedFilm" :film="selectedFilm"></film-detail>
    </div>
    <favourite-list :favourites="favourites">{{favourites}}</favourite-list>
    
  </div>
</template>

<script>

import {eventBus} from './main.js';
// import FilmList from './components/FilmList.vue';
import FilmDetail from './components/FilmDetail.vue';
import FavouriteList from './components/FavouriteList.vue';

export default {
  name: 'App',
  data(){
    return {
      films:[],
      selectedFilm: null,

    }
  },
  components: {
    // 'film-list': FilmList,
    'film-detail': FilmDetail,
    'favourite-list': FavouriteList,
  },
  computed: {
    favourites: function() {
      console.log(this.films);
      return this.films.filter(film => film.isFavourite);
    }
  },
  methods:{
    getFilms: function(){
      fetch('https://ghibliapi.herokuapp.com/films')
      .then(res => res.json())
      .then(films => this.films = films)
    },
    markFavourite: function(film){
      // console.log("duhaushduahsduahsud");
      const index = this.films.indexOf(film);
      this.films[index].isFavourite = true;
    },
    unmarkFavourite: function(film){
      const index = this.films.indexOf(film);
      this.films[index].isFavourite = false;
    }
  },
  mounted(){
    this.getFilms();
    eventBus.$on('film-selected', film => (this.selectedFilm = film));
    eventBus.$on('favourite-added', film => this.markFavourite(film));
    eventBus.$on('favourite-removed', film => this.unmarkFavourite(film));
  }
};
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
