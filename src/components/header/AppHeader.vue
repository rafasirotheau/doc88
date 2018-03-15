<template>
  <header>
    <div class="container">
      <div class="grid-xs-3 visible-xs visible-sm">
        <button class="gradient-button" type="button"><font-awesome-icon icon="bars" /></button>
      </div>
      <div class="grid-xs-6 grid-md-3">
        <img src="/static/img/pinball-logo.png">
      </div>
      <div class="grid-xs-3 visible-xs visible-sm text-right">
        <button class="gradient-button" type="button"><font-awesome-icon icon="search" /></button>
      </div>
      <div class="grid-xs-6 visible-md">
        <button class="gradient-button bars" type="button"><font-awesome-icon icon="bars" /></button>
        <form :submit="filterIt" id="search-form">
          <input type="text" name="" value="" v-model="movieFilter"/>
          <button type="submit"><font-awesome-icon icon="search" /></button>
        </form>
      </div>
      <div class="grid-xs-3 visible-md">
        <div class="user-info">
          <span class="image">
            <img src="/static/img/users/john.jpg" alt="">
          </span> Welcome John
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
export default {
  name: 'HeaderMenu',

  components: { FontAwesomeIcon },

  methods: {
    filterIt () {
      console.log(this.movieFilter)
    }
  },

  data () {
    return {
      movieFilter: ''
    }
  }
}
</script>

<style src="./AppHeader.scss" scoped lang="scss"></style>
