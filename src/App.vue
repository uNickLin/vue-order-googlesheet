<template lang='pug'>
  #app
    Navbar
    router-view
    Footer
</template>

<script>
import Navbar from '@/components/template/Navbar'
import Footer from '@/components/template/Footer'

export default {
  name: 'App',
  components: {
    Navbar,
    Footer
  }
}
</script>

<style lang='scss'>
@import '../src/assets/scss/app.scss';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
