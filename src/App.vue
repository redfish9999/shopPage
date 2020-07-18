<template>
  <div id="app">
      <Cart></Cart>
  </div>
</template>

<script>
import Cart from './pages/Cart.vue'

export default {
  name: 'app',
  components: {
    Cart
  }
}
</script>

<style>
html {
  background-color: darkgray;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 30px;
  
  position: absolute;
  left: 50%;
  margin-left: -300px;
}
</style>
