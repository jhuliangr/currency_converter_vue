<template >
    <Currency/>
</template>

<script>
import Currency from './pages/currency.vue'
import 'bootstrap/dist/css/bootstrap.css'

export default {
  name: 'App',
  components: {
    Currency
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin-top: 60px;
}
body {
  background: linear-gradient(to left, rgba(13, 37, 0, 0.9), rgba(242, 255, 204, 0.8))
}
</style>
