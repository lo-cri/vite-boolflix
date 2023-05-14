<script>

import axios from 'axios';
import { store } from './store';
import Search from './components/Search.vue';
import CardsDefault from './components/CardsDefault.vue';
import CardsSearch from './components/CardsSearch.vue';

const API_KEY = '4da7dcca1d0417941fc8e999936345fe'

export default {
  name: "App",
  components: {
    Search,
    CardsDefault,
    CardsSearch,
  },

  data() {
    return {
      store
    }
  },

  methods: {

    movieApi() {
      axios.get(`https://api.themoviedb.org/3/search/multi?api_key=${API_KEY}&language=it-IT&query=${store.film}`).then((res) => {
        return this.store.movieArray = res.data
      })
    },

  }
}

</script>

<template>
  <div id="container">
    <Search @filmChange="movieApi()" />
    <CardsDefault :class="(store.film != '' && typeof store.movieArray.results != 'undefined') ? 'd-none' : 'd-block'" />
    <CardsSearch />
  </div>
</template>

<style>
@import '../node_modules/bootstrap/dist/css/bootstrap.css';

#container {
  background-color: rgb(20, 20, 20);
  color: rgba(255, 255, 255, 0.8);
}
</style>
