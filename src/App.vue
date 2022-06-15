<template>
  <div id="app">
    <vue-form-generator />
  </div>
</template>

<script>
import VueFormGenerator from './components/VueFormGenerator';

export default {
  name: 'App',
  components: {
    VueFormGenerator    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 10vh 20vw;
}
</style>
