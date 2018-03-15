<template>
  <div class="movie-list"  v-infinite-scroll="loadMovies" infinite-scroll-disabled="reachedEnd">
    <div class="movie-tile" v-for="(movie, index) in movieListArray" :key="index">
      <movie-card :movie="movie"/>
    </div>
  </div>
</template>

<script>
import MovieCard from '@/components/MovieCard'
import infiniteScroll from 'vue-infinite-scroll'
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

  components: { MovieCard },

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
