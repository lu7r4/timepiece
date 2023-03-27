<template>
  <div id="app">
    <Timepiece />
  </div>
</template>

<script>
import Timepiece from './components/Timepiece.vue'

export default {
  name: 'App',
  components: {
    Timepiece
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-color: #353638;
  min-height: 100vh;
  font-family: 'Gotham Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
