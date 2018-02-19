<template lang="pug">
  #app
    img(src='zylonzmusic/dist/logo.png')
    h1 {{ title }}
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    ul
      // li(v-for="artist in artists") {{artist.name}}
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
    h2 Essential Links
    spinner(v-show="loading")
    ul
      li: a(href='https://vuejs.org', target='_blank') Core Docs
      li: a(href='https://forum.vuejs.org', target='_blank') Forum
      li: a(href='https://chat.vuejs.org', target='_blank') Community Chat
      li: a(href='https://twitter.com/vuejs', target='_blank') Twitter
    h2 Ecosystem
    ul
      li: a(href='http://router.vuejs.org/', target='_blank') vue-router
      li: a(href='http://vuex.vuejs.org/', target='_blank') vuex
      li: a(href='http://vue-loader.vuejs.org/', target='_blank') vue-loader
      li: a(href='https://github.com/vuejs/awesome-vue', target='_blank') awesome-vue
</template>

<script>
import Artist from './components/Artist.vue';
import Spinner from './components/Spinner.vue';
import getArtists from './api';

export default {
  name: 'app',
  data () {
    return {
      title: 'Welcome to the project "Top artists by country" with Vue.js',
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
        {name: 'Perú', value: 'peru'},
        {name: 'Venezuela', value: 'venezuela'}
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
    getArtistsByComponent(){
      this.loading = true;
      this.artists = [];
      getArtists(this.selectedCountry)
        .then((artists) => { this.artists = artists })
        .catch(error => console.error(error))
        .finally(() => this.loading = false );
    }
  },
  mounted: function(){
    this.getArtistsByComponent();
  },
  watch: {
    selectedCountry: function(){
      this.getArtistsByComponent();
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  overflow hidden
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
