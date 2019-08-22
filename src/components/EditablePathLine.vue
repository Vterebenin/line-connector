<template>
  <g>
    <path
      :d="'M ' + from.x + ' ' + from.y + ' C ' + bezierFrom.x + ' ' + bezierFrom.y + ', ' + bezierTo.x + ' ' + bezierTo.y + ', ' + to.x + ' ' + to.y"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 1 -->
    <path
      :d="'M ' + from.x + ' ' + from.y + ' ' + bezierFrom.x + ' ' + bezierFrom.y"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 2 -->
    <path
      :d="'M ' + to.x + ' ' + to.y + ' ' + bezierTo.x + ' ' + bezierTo.y"
      stroke="black"
      fill="transparent"
    />
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
  </g>
</template>

<script>
export default {
  name: "EditablePathLine",
  props: {
    position: Object,
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
</style>
