<template>
  <div class="movie-list"  v-infinite-scroll="loadMovies" infinite-scroll-disabled="reachedEnd">
    <masonry :cols="{ default: 4, 1100: 3, 840: 2, 540: 1 }" :gutter="20">
      <div class="movie-tile" v-for="(movie, index) in movieListArray" :key="index">
        <movie-card :movie="movie"/>
      </div>
    </masonry>
    <div class="loading" v-show="showLoading">
      <font-awesome-icon icon="spinner" spin /> Page Loading
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import MovieCard from '@/components/MovieCard'
import infiniteScroll from 'vue-infinite-scroll'
import VueMasonry from 'vue-masonry-css'

Vue.use(VueMasonry)

const movieApi = {
  base: 'https://api.themoviedb.org/3/discover/movie',
  params: {
    'api_key': 'da1f62f1e48c5a9ecd177dfb3b1a15e5',
    'language': 'pt-BR',
    'include_adult': false,
    'sort_by': 'vote_average.desc',
    'vote_count.gte': 2000,
    'with_genres': '16'
  },
  url: function (page) {
    let params = []

    this.params.page = page || this.currentPage

    Object.keys(this.params).forEach((k) => {
      params.push(k + '=' + this.params[k])
    })
    return this.base + '?' + params.join('&')
  }
}

export default {
  name: 'MovieList',

  components: { FontAwesomeIcon, MovieCard },

  directives: { infiniteScroll },

  methods: {
    loadMovies: function () {
      var xhr = new XMLHttpRequest()
      var self = this
      self.currentPage++

      self.showLoading = true

      xhr.open('GET', movieApi.url(self.currentPage))
      xhr.onload = function () {
        let results = JSON.parse(xhr.responseText).results
        self.movieListArray = self.movieListArray.concat(results)

        self.showLoading = false
      }

      xhr.send()

      if (self.currentPage >= self.limitPage) {
        self.reachedEnd = true
      }
    }
  },

  data () {
    return {
      showLoading: true,
      reachedEnd: false,
      currentPage: 0,
      limitPage: 5,
      movieListArray: []
    }
  }
}
</script>

<style src="./MovieList.scss" scoped lang="scss"></style>
