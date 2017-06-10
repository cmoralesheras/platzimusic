<template lang="pug">
  #app
    img(src='dist/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry") 
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtist from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries : [
        {name: 'Argentina', value:"argentina"},
        {name: 'Mexico', value:"mexico"},
        {name: 'España', value:"spain"}
      ],
      selectedCountry: 'argentina',
      loading : true
    }
  },
  methods: {
    refreshArtist(){
      const self = this
      this.loading = true
      this.artists = []
      getArtist(this.selectedCountry)
        .then(function(artists){
          self.loading = false
          self.artists = artists
        })
    }
  },
  components: {
    Artist,
    Spinner
  },
  mounted(){
    this.refreshArtist()
  },
  watch:{
    selectedCountry(){
      this.refreshArtist()
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
    color red
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
