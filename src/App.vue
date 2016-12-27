<template>
  <div id="app">
    <MyHeader></MyHeader>
    <Weather></Weather>
    <forecast></forecast>
  </div>
</template>

<script>
import MyHeader from './components/my-header'
import Weather from './components/weather'
import Forecast from './components/forecast'

export default {
  name: 'app',
  components: {
    MyHeader,
    Weather,
    Forecast
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
