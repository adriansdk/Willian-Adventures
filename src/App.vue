<template>
  <div id="app">
    <Amigo> </Amigo>
  </div>
</template>

<script>
import Amigo from './components/Amigo.vue'

export default {
  name: 'app',
  components: {
    Amigo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f3f3f3;
  text-shadow: 2px 2px black;
  height: 1500px;
  background-image: url('https://i.redd.it/xwvzch6jabwy.png');
  background-size: cover;
}
</style>
