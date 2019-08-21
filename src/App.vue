<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<template>
  <div id="app">
    <HelloWorld msg="sadfsadf" someotherthing="true" />
  </div>
</template>

<style>
body {
  overflow-x: hidden;
}
#app {
  text-align: center;
}
</style>
