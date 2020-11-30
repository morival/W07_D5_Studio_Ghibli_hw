<template>
  <div id="app">
    <div class="logo">
      <h1>Studio Ghibli</h1>
      <h2 v-if="!films.length">Loading...</h2>
    </div>
    <div class="main-content">
      <div class="left-bar">
        <div id="list-dropdown" v-if="films.length">
          <label for="film_select">Select a Film:</label>
          <select id="film_select" v-model="selectedFilm">
            <option disabled value="">Select a film</option>
            <option v-for="film in films" :key="film.id" :value="film"> {{film.title}} </option>
          </select>
        </div>
    <!-- buttons -->
        <button v-on:click="showFilter = !showFilter">Filter Films by Year</button>
        <button v-on:click="showFavourites = !showFavourites">Show Favourite Films</button>
    <!-- filter input -->
        <div id="filteredInput" v-show="!showFilter">
          <h3>Filter Films by Year</h3>
          <p>from year:</p>
          <input type="number" min="1986" max="2020" v-model.number="fromYear"/>
          <p>until year:</p>
          <input type="number" min="1986" max="2020" v-model.number="untilYear"/>
        </div>
    <!-- filter results -->
        <section v-show="!showFilter">
          <div class="film-by-year" v-for="(film, index) in filteredYear" :key="index">
            <h5> {{film.title}}</h5>
            <h6> Year {{film.release_date}} </h6>
          </div>
        </section>
      </div>
    <!-- film details -->
      <div class="main-container">
        <div>
          <film-detail v-if="selectedFilm" :film="selectedFilm"></film-detail>
        </div>
    <!-- favourites -->
        <div v-show="!showFavourites">
          <favourite-list :favourites="favourites"></favourite-list>
        </div>
      </div>
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
      showFilter: true,
      showFavourites: true
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
    },
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
  color: white;
  padding: 30px;
  background-color: #2c3e50;
}
/* .main-content {
  display: grid;
  grid-template-columns: 1fr 3fr;
} */
.left-bar {
  width: 600px;
  height: 600px;
  overflow-y:  scroll;
  
}
</style>
