<template>
  <div class="movie-card">
    <div :class="randomSizeClass()" :style="{ backgroundImage: 'url(https://image.tmdb.org/t/p/w300_and_h450_bestv2' + movie.backdrop_path + ')' }"></div>
    <img :src="'https://image.tmdb.org/t/p/w780' + movie.backdrop_path">
    <div class="movie-card--content">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.overview | truncate(12) }}</p>
    </div>
    <div class="movie-card--footer">
    </div>
  </div>
</template>

<script>
export default {
  name: 'MovieCard',
  props: ['movie'],

  filters: {
    truncate: function (text, size) {
      var words = text.split(/\s/)
      var truncated = words.slice(0, size).join(' ')
      return truncated + (size < words.length ? '...' : '')
    }
  },

  methods: {
    randomSizeClass: function () {
      const sizes = [152, 243, 158, 316]

      return 'image-' + sizes[Math.floor(Math.random() * sizes.length)]
    },
    fourStarRating: function () {
      return this.movie.vote_average * 4 / 10
    }
  }
}
</script>

<style src="./MovieCard.scss" scoped lang="scss"></style>
