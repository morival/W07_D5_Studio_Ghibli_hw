<template lang="html">
    <div class="film-list" v-if="films.length">
        <listed-film v-for="(film, index) in films" :film="film" :key="index"></listed-film>
    </div>
</template>

<script>
import ListedFilm from '@/components/ListedFilm.vue';

export default {
    name: 'film-list',
    props: ['films'],
    components: {
        'listed-film': ListedFilm
    }
};
</script>

<style>

</style>