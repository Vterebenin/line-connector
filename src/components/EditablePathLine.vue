<template>
  <g>
    <path
      :d="`M ${from.x} ${from.y} C ${bezierFrom.x} ${bezierFrom.y}, ${bezierTo.x} ${bezierTo.y}, ${to.x}  ${to.y}`"
      stroke="black"
      fill="transparent"
      marker-start="url(#UpArrowPrecise)"
      marker-end="url(#DownArrowPrecise)"
    />

    <!-- безье хелпер 1 -->
    <path
      :d="`M ${from.x} ${from.y} ${bezierFrom.x} ${bezierFrom.y}`"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 2 -->
    <path :d="`M ${to.x} ${to.y} ${bezierTo.x} ${bezierTo.y}`" stroke="black" fill="transparent" />
    <!-- точка для хелпера 1 -->
    <circle
      :cx="bezierFrom.x"
      :cy="bezierFrom.y"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('bezierFrom')"
      r="5"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <!-- точка для хелпера2 -->
    <circle
      :cx="bezierTo.x"
      :cy="bezierTo.y"
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
      :cx="from.x"
      :cy="from.y"
      r="20"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('from')"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <!-- большой кружок -->
    <circle
      class="drawer__circle drawer__circle-2"
      :cx="to.x"
      :cy="to.y"
      r="20"
      v-on:dragstart="dragstartFalse"
      v-on:mousedown="dragElement('to')"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <marker id="UpArrowPrecise" markerWidth="40" markerHeight="10" refX="0" refY="7" orient="auto">
      <path d="M30,0 L30,10 L20,5 Z" />
    </marker>

    <marker
      id="DownArrowPrecise"
      markerWidth="20"
      markerHeight="10"
      refX="10"
      refY="5"
      orient="auto"
    >
      <path d="M0,0 L0,10 L20,5 Z" />
    </marker>
  </g>
</template>

<script>
export default {
  name: "EditablePathLine",
  props: {
    position: Object
  },
  data: function() {
    return {
      from: {
        x: this.position.x1,
        y: this.position.y1
      },
      to: {
        x: this.position.x2,
        y: this.position.y2
      },
      bezierFrom: {
        x: this.position.x1,
        y: this.position.y2
      },
      bezierTo: {
        x: this.position.x2,
        y: this.position.y1
      }
    };
  },
  computed: {
    arrowPos: {
      get: function() {
        return {
          x: this.bezierFrom.x - this.from.x + 10,
          y: this.bezierFrom.x - this.from.y + 10
        };
      }
    }
  },
  mounted: function() {
    // axios.get('symbols.json').then(response => this.symbols = response.data);
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
        reducedThis[changer] = {
          x: parseFloat(pageX - shiftX),
          y: parseFloat(pageY - shiftY)
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
</style>
