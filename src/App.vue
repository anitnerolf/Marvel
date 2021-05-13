<template>
  <div id="app">
    <img alt="logo" src="./assets/logo.png" width="50%">
    <Marvel msg="Welcome to Marvel Characters"/>
  </div>
</template>

<script>
import Marvel from './components/Marvel.vue'

export default {
  name: 'App',
  components: {
    Marvel
  }
}
</script>

<style lang="scss">
#app {
  background-color: #020202;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 60px;
}
</style>
