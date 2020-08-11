<template>
  <div id="app">
    <countries></countries>
  </div>
</template>

<script>
import countries from './components/Countries.vue'

export default {
  name: 'App',
  components: {
    'countries': countries
  }
}
</script>

<style>
@import url('https://cdn.rawgit.com/mfd/09b70eb47474836f25a21660282ce0fd/raw/e06a670afcb2b861ed2ac4a1ef752d062ef6b46b/Gilroy.css');

* {
  box-sizing: border-box;
  margin: 0;
}

body {
  margin: 0;
  padding: 0;
  background: #fefefe !important;
}

#app {
  font-family: 'Gilroy', sans-serif;
  color: #000;
}
</style>
