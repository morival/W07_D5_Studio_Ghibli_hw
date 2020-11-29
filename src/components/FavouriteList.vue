<template>
  <div id="favourite-list">
      <h3>Your Favourites</h3>
      <ul>
          <favourite-film v-for="(film, index) in favouriteFilms" :film="film" :key="index">

          </favourite-film>
      </ul>
  </div>
</template>

<script>
import FavouriteFilm from '@/components/FavouriteFilm.vue';

export default {
    name: 'favourite-list',
    props: ['favouriteFilms'],
    components: {
        'favourite-film': FavouriteFilm
    },
}
</script>

<style>

</style>