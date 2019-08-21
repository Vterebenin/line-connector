<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      {{ from.x }}
      <svg id="test" class="drawer" height="100%" width="100%">
        <path v-bind:d="'M ' + from.x + ' ' + from.y + ' C 120 140, 180 140, ' + to.x + ' ' + to.y" stroke="black" fill="transparent" />
        <circle
          class="drawer__circle drawer__circle-1"
          cx="50"
          cy="50"
          r="40"
          v-on:dragstart="dragstartFalse"
          v-on:mousedown="mouseDownOnCircle($event)"
          stroke="black"
          stroke-width="3"
          fill="red"
        />
        <circle
          class="drawer__circle drawer__circle-2"
          cx="150"
          cy="150"
          r="80"
          v-on:dragstart="dragstartFalse"
          v-on:mousedown="mouseDownOnCircle($event)"
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
  data: function() {
    return {
      time: 0,
      from: {
        x: 0,
        y: 0
      },
      to: {
        x: 0,
        y: 0
      }
    }
  },

  props: {
    msg: String,
    someotherthing: String
  },
  computed: {
    pathFrom: {
      get: function(newVal) {
        return (this.from = { x: 0, y: 0 });
      },
      set: function(newVal) {
        return (this.from = newVal);
      }
    },
    pathTo: {
      get: function(newVal) {
        return (this.to = { x: 0, y: 0 });
      },
      set: function(newVal) {
        return (this.to = newVal);
      }
    }
    // from: function(params) {}
  },
  created: function() {
    // axios.get('symbols.json').then(response => this.symbols = response.data);
      this.time = Date.now
    console.log("test")
  },
  methods: {
    debugMessage() {},
    mouseDownOnCircle($event) {
      let ball = event.target;
      let ball1 = document.getElementsByClassName("drawer__circle-1")[0]
      
      let ball2 = document.getElementsByClassName("drawer__circle-2")[0]
      let radius = ball.getAttribute("r");
      let svg = document.getElementById("test");
      const reducedThis = this;

      let shiftX = event.clientX - ball.getBoundingClientRect().left - radius;
      let shiftY = event.clientY - ball.getBoundingClientRect().top - radius;

      svg.append(ball);

      moveAt(event.pageX, event.pageY, reducedThis);

      // moves the ball at (pageX, pageY) coordinates
      // taking initial shifts into account
      function moveAt(pageX, pageY, reducedThis) {
        ball.setAttribute("cx", pageX - shiftX);
        ball.setAttribute("cy", pageY - shiftY);
        console.log(reducedThis)
        reducedThis.pathFrom = {
          x: ball1.getAttribute("cx"),
          y: ball1.getAttribute("cy")
        }
        reducedThis.pathTo = {
          x: ball2.getAttribute("cx"),
          y: ball2.getAttribute("cy")
        }
      }

      function onMouseMove(event) {
        moveAt(event.pageX, event.pageY, reducedThis);
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
.drawer {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
