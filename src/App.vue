<template>
  <div id="app">
    <img src="https://kirlled.github.io/curso-vuejs/src/assets/logo.png">
    <h1>PlatziMusic</h1>
    
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value">{{ country.name }}</option>
    </select>

    <spinner v-show="loading"></spinner>

    <ul>
      <artist v-for="artist in artists" :artist="artist" :key="artist.mbid"></artist>
    </ul>

  </div>
</template>

<script>

import getArtists from './api/index';
import Spinner from './components/Spinner.vue';
import Artist from './components/Artist.vue';

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina' },
        { name: 'España', value: 'spain' },
        { name: 'Colombia', value: 'colombia' },
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtist() {
      const self = this;
      this.loading = true;
      this.artists = [];
      
      getArtists(this.selectedCountry)
        .then(function (artists){
          self.loading = false;
          self.artists = artists;
        });
    }
  },
  mounted() {
    this.refreshArtist();
  },
  watch: {
    selectedCountry() {
      this.refreshArtist();
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
