<template>
  <g>
    <path
      :d="`M ${to.x} ${to.y}  C ${bezierFrom.x} ${bezierFrom.y}, ${bezierTo.x} ${bezierTo.y}, ${from.x}  ${from.y} `"
      stroke="black"
      :id="`path${pathId}`"
      fill="transparent"
    />

    <!-- безье хелпер 1 -->
    <path
      :d="`M ${to.x} ${to.y} ${bezierFrom.x} ${bezierFrom.y}`"
      stroke="black"
      fill="transparent"
    />
    <!-- безье хелпер 2 -->
    <path :d="`M ${from.x} ${from.y} ${bezierTo.x} ${bezierTo.y}`" stroke="black" fill="transparent" />
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
 
    <text class="b-arrow">
      <textPath :xlink:href="`#path${pathId}`"  startOffset="20px" text-anchor="right" >🢀</textPath>
    </text>
    <text class="b-text">
      <textPath :xlink:href="`#path${pathId}`" startOffset="50%" text-anchor="middle">1test text</textPath>
    </text>
    
  </g>
</template>

<script>
export default {
  name: "EditablePathLine",
  props: {
    position: Object,
    pathId: Number
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
text {
  font-size: 14px;
  fill: #000;
  
  /* transform: rotate(10deg); */
  dominant-baseline: central;
   -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
}
text.b-text {
  font-size: 20px;
  dominant-baseline: ideographic;
}
</style>
