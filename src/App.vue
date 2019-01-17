<template>
  <div id="readerApp">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'readerApp',
  components: {
    HelloWorld
  }
}
</script>

<style>
#readerApp {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  transition: background 0.2s linear 0s, font-size 0.2s linear 0s, font-family 0.2s linear 0s, color 0.2s linear 0s;
}
</style>
