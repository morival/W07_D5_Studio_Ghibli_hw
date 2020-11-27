<template>
  <div id="selected-film" v-if="film">
      <h2> {{film.title}} ({{film.release_date}})</h2>
      <p> {{film.description}} </p>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'film-detail',
    props: ['film'],
    methods: {

    }
}
</script>

<style>

</style>