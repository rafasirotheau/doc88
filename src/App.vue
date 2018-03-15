<template>
  <div id="app">
    <app-header/>
    <div class="content">
      <router-view/>
    </div>
  </div>
</template>

<script>
import AppHeader from '@/components/header/AppHeader'
export default {
  name: 'App',
  components: { AppHeader }
}
</script>

<style src="./App.scss" lang="scss"></style>
