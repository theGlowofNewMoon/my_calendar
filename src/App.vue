<template>
  <calendar></calendar>
</template>

<script>
import calendar from './components/calendar'
export default {
  name: 'App',
  components: {
    calendar
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
ul{
  padding: 0;
  margin: 0;
}
li{
  list-style: none;
}
</style>
