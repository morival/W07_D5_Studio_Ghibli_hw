<template>
  <li v-on:click="handleClick"> {{film.title}} </li>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'listed-film',
    props: ['film'],
    methods: {
        handleClick(){
            eventBus.$emit('beer-selected', this.film)
        }
    }
}
</script>

<style>

</style>