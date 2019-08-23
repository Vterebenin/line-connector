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
      <EditablePathLine :pathId="1" :position="{x1: 50, y1: 150, x2: 200, y2: 350 }" msg="sadfsadf" someotherthing="true" />
      <EditablePathLine :pathId="2" :position="{x1: 600, y1: 150, x2: 800, y2: 350 }" msg="sadfsadf" someotherthing="true" />
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
