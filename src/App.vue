<template>
  <div id="app">
    <Phone/>
  </div>
</template>

<script>
import Phone from './components/Phone'

export default {
  name: 'App',
  components: {
    Phone
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
