<template>
  <div id="app">
    <wheel/>
  </div>
</template>

<script>
import wheel from './components/wheel'

export default {
  name: 'App',
  components: {
    wheel
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
