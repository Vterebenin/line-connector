<template>
  <g>
    <path
      v-bind:d="'M ' + from.x + ' ' + from.y + ' C ' + bezierFrom.x + ' ' + bezierFrom.y + ', ' + bezierTo.x + ' ' + bezierTo.y + ', ' + to.x + ' ' + to.y"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 1 -->
    <path
      v-bind:d="'M ' + from.x + ' ' + from.y + ' ' + bezierFrom.x + ' ' + bezierFrom.y"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 2 -->
    <path
      v-bind:d="'M ' + to.x + ' ' + to.y + ' ' + bezierTo.x + ' ' + bezierTo.y"
      stroke="black"
      fill="transparent"
    />
    <!-- точка для хелпера 1 -->
    <circle
      v-bind:cx="bezierFrom.x"
      v-bind:cy="bezierFrom.y"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('bezierFrom')"
      r="5"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <!-- точка для хелпера2 -->
    <circle
      v-bind:cx="bezierTo.x"
      v-bind:cy="bezierTo.y"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('bezierTo')"
      r="5"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <!-- маленький кружок -->
    <circle
      class="drawer__circle drawer__circle-1"
      v-bind:cx="from.x"
      v-bind:cy="from.y"
      r="20"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('pathFrom')"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <!-- большой кружок -->
    <circle
      class="drawer__circle drawer__circle-2"
      v-bind:cx="to.x"
      v-bind:cy="to.y"
      r="20"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('pathTo')"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
  </g>
</template>

<script>
export default {
  name: "EditablePathLine",
  data: function() {
    return {
      from: {
        x: 150,
        y: 400
      },
      to: {
        x: 450,
        y: 400
      },
      bezierFrom: {
        x: 120,
        y: 140
      },
      bezierTo: {
        x: 450,
        y: 140
      }
    };
  },

  props: {
    msg: String,
    someotherthing: String
  },
  computed: {
    pathFrom: {
      get: function() {
        return this.from;
      },
      set: function(newVal) {
        return (this.from = newVal);
      }
    },
    pathTo: {
      get: function() {
        return this.to;
      },
      set: function(newVal) {
        return (this.to = newVal);
      }
    },
    bezierFromC: {
      get: function() {
        return this.bezierFrom;
      },
      set: function(newVal) {
        return (this.bezierFrom = newVal);
      }
    },
    bezierToC: {
      get: function() {
        return this.bezierTo;
      },
      set: function(newVal) {
        return (this.bezierTo = newVal);
      }
    }
    // from: function(params) {}
  },
  created: function() {
    // axios.get('symbols.json').then(response => this.symbols = response.data);
    this.time = Date.now;
  },
  methods: {
    dragElement(changer) {
      let ball = event.target;
      let radius = ball.getAttribute("r");
      let svg = document.getElementById("test");

      let shiftX = event.clientX - ball.getBoundingClientRect().left - radius;
      let shiftY = event.clientY - ball.getBoundingClientRect().top - radius;
      const reducedThis = this;

      svg.append(ball);

      moveAt(event.pageX, event.pageY, reducedThis, changer);

      // moves the ball at (pageX, pageY) coordinates
      // taking initial shifts into account
      function moveAt(pageX, pageY, reducedThis, changer) {
        ball.setAttribute("cx", pageX - shiftX);
        ball.setAttribute("cy", pageY - shiftY);

        reducedThis[changer] = {
          x: ball.getAttribute("cx"),
          y: ball.getAttribute("cy")
        };
      }

      function onMouseMove(event) {
        moveAt(event.pageX, event.pageY, reducedThis, changer);
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
