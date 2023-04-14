<template>
  <SpecWriter msg="Welcome to Your Vue.js App"/>
</template>

<script>
import SpecWriter from './components/SpecflowWriter.vue'

export default {
  name: 'App',
  components: {
    SpecWriter
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
