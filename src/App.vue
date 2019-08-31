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
      <EditablePathLine
        :text="`test1`"
        :pathId="1"
        :radius="20"
        :position="{x1: 50, y1: 150, x2: 200, y2: 350 }"
        msg="sadfsadf"
        someotherthing="true"
      />
      <EditablePathLine
        :text="`test2`"
        :pathId="2"
        :radius="40"
        :position="{x1: 600, y1: 150, x2: 800, y2: 350 }"
        msg="sadfsadf"
        someotherthing="true"
      />
    </svg>
    
  </div>
</template>

<style>
body {
  overflow-x: hidden;
  margin: 0;
  padding: 0;
  /* background: yellow; */
}
#app {
  text-align: center;
  height: 900px;
  position: absolute;
  left: 0;
  right: 0;
}
</style>
