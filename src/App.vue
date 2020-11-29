<template>
  <div id="app">
    <h1>Studio Ghibli</h1>
    <h2 v-if="!films.length">Loading...</h2>
    <div id="list-dropdown" v-if="films.length">
      <label for="film_select">Select a Film:</label>
      <select id="film_select" v-model="selectedFilm">
        <option disabled value="">Select a film</option>
        <option v-for="film in films" :key="film.id" :value="film"> {{film.title}} </option>
      </select>
    </div>
    <div id="filteredInput">
      <h3>Filter Films by Year</h3>
      <input type="number" v-model.number="fromYear"/>
      <input type="number" v-model.number="untilYear"/>
    </div>
    <section>
      <div class="film-by-year" v-for="(film, index) in filteredYear" :key="index">
        <h4> {{film.title}} </h4>
        <p> Year {{film.release_date}} </p>
      </div>
    </section>


    <!-- <div id="list-info" v-if="films.length">
      <film-list :films="films"></film-list>
    </div> -->

    <div class="main-container">
      <film-detail v-if="selectedFilm" :film="selectedFilm"></film-detail>
    </div>
    <div>
      <favourite-list :favourites="favourites"></favourite-list>
    </div>
    
    
  </div>
</template>

<script>

import {eventBus} from './main.js';
import FilmList from './components/FilmList.vue';
import FilmDetail from './components/FilmDetail.vue';
import FavouriteList from './components/FavouriteList.vue';

export default {
  name: 'App',
  data(){
    return {
      films:[],
      selectedFilm: null,
      fromYear: 1986,
      untilYear: 2020,
    }
  },
  components: {
    'film-list': FilmList,
    'film-detail': FilmDetail,
    'favourite-list': FavouriteList,
  },
  computed: {
    favourites: function(){
      return this.films.filter(film => film.isFavourite);
    },
    filteredYearFrom: function(){
      return this.films.filter((film) => {
        return parseInt(film.release_date) >= this.fromYear;
      });
    },
    filteredYearUntil: function(){
      return this.films.filter((film) => {
        return parseInt(film.release_date) <= this.untilYear;
      });
    },
    filteredYear: function(){
      const filterPromise = this.filteredYearFrom.filter((f) => {
        return this.filteredYearUntil.some((u) => {
          return f.release_date === u.release_date;
        })
      });
      return filterPromise;
    }
  },
  methods:{
    getFilms: function(){
      fetch('https://ghibliapi.herokuapp.com/films')
        .then(res => res.json())
        .then(filmData => {
          filmData.forEach(film => (film.isFavourite = false));
          this.films = filmData;
        })
    },
    markFavourite: function(film){
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
