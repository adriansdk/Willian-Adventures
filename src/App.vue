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
  font-family: 'Lobster', cursive;
  text-align: center;
  width: 100%;
  height: 1050px;
  color: #f3f3f3;
  text-shadow: 2px 2px black;
  background-image: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url(https://i.redd.it/xwvzch6jabwy.png);
  background-size: cover;
  background-position: bottom;
}
</style>
