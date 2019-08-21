<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      {{ someotherthing }}
      <svg id="test" style="position: absolute; left: 0; top: 0;" height="100%" width="100%" >
        <circle
          cx="50"
          cy="50"
          r="40"
          v-on:dragstart="dragstartFalse"
          v-on:mousedown="mousedownn($event)"
          stroke="black"
          stroke-width="3"
          fill="red"
        />
      </svg>
    </p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
    someotherthing: String
  },
  methods: {
    debugMessage() {
    },
    mousedownn($event) {
      let ball = event.target;
      let radius = ball.getAttribute("r")
      console.log(radius)
      let svg = document.getElementById("test")
      console.log(ball.getBoundingClientRect().left)

      let shiftX = event.clientX - ball.getBoundingClientRect().left - radius;
      let shiftY = event.clientY - ball.getBoundingClientRect().top  - radius

      svg.append(ball);

      moveAt(event.pageX, event.pageY);

      // moves the ball at (pageX, pageY) coordinates
      // taking initial shifts into account
      function moveAt(pageX, pageY) {
        ball.setAttribute("cx", pageX - shiftX);
        ball.setAttribute("cy", pageY - shiftY);
      }

      function onMouseMove(event) {
        moveAt(event.pageX, event.pageY);
      }

      // move the ball on mousemove
      document.addEventListener("mousemove", onMouseMove);

      // drop the ball, remove unneeded handlers
      ball.onmouseup = function() {
        document.removeEventListener("mousemove", onMouseMove);
        ball.onmouseup = null;
      };
    },
    dragstartFalse() {
      return false;
    }
  }
};
</script>

<style scoped>

</style>
