<template>
  <div class="my-header">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Weather App Based on Vue.js'
    }
  }
}
</script>

<style scoped>
p {
  text-align: center;
  font-size: 30pt;
}
</style>
