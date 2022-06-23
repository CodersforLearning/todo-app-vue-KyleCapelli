<template>
  <div id="app">
    <Tasks />
  </div>
</template>

<script>
import Tasks from './components/Tasks.vue'

export default {
  name: 'App',
  components: {
    Tasks
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@400;700&display=swap');

#app {
  font-family: 'Josefin Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
