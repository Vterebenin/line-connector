<script>
import EditablePathLine from "./components/EditablePathLine.vue";

export default {
  name: "app",
  components: {
    EditablePathLine
  }
};
</script>

<template>
  <div id="app">
    <svg id="test" class="drawer" height="100%" width="100%">
      <EditablePathLine position msg="sadfsadf" someotherthing="true" />
      <EditablePathLine position msg="sadfsadf" someotherthing="true" />
    </svg>
  </div>
</template>

<style>
body {
  overflow-x: hidden;
  margin: 0;
  padding: 0;
}
#app {
  text-align: center;
  height: 100vh;
  position: absolute;
  left: 0;
  right: 0;
}
</style>
