<template>
  <g>
    <defs>
      <marker
        id="someArrowStart"
        markerWidth="20"
        markerHeight="20"
        refX="0"
        refY="5"
        orient="auto"
      >
        <path
          d="M 0,5 L 10,10 L 10,0 Z"
          style="pointer-events: none;"
          @mousedown="dragElement('bezierToStart', true, '', 'to')"
          fill="blue"
        />
      </marker>
      <marker id="someArrowEnd" markerWidth="20" markerHeight="20" refX="10" refY="5" orient="auto">
        <path
          d="M 0,0 L 0,10 L 10,5 Z"
          style="pointer-events: none;"
          @mousedown="dragElement('bezierFromStart', true, '', 'from')"
          fill="blue"
        />
      </marker>
    </defs>
    <!-- главный паф -->
    <path
      :d="`M ${bezierFromStart.x} ${bezierFromStart.y}  C ${bezierFrom.x} ${bezierFrom.y}, ${bezierTo.x} ${bezierTo.y}, ${bezierToStart.x}  ${bezierToStart.y} `"
      marker-start="url(#someArrowStart)"
      marker-end="url(#someArrowEnd)"
      stroke="black"
      :id="`path${pathId}`"
      fill="transparent"
    />

    <!-- безье хелпер 1 -->
    <path
      :d="`M ${bezierFromStart.x} ${bezierFromStart.y} ${bezierFrom.x} ${bezierFrom.y}`"
      stroke="black"
      stroke-dasharray="5,5"
      fill="transparent"
    />
    <!-- безье хелпер 2 -->
    <path
      :d="`M ${bezierToStart.x} ${bezierToStart.y} ${bezierTo.x} ${bezierTo.y}`"
      stroke-dasharray="5,5"
      stroke="black"
      fill="transparent"
    />

    <!-- первый кружок -->
    <circle
      class="drawer__circle grabable drawer__circle-1"
      :id="`circle${pathId}`"
      :cx="from.x"
      :cy="from.y"
      :r="`${r}`"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('from', false, 'bezierFromStart')"
      stroke="black"
      stroke-width="1"
      fill="transparent"
    />
    <!-- второй кружок -->
    <circle
      class="drawer__circle grabable drawer__circle-2"
      :cx="to.x"
      :cy="to.y"
      :r="`${r}`"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('to', false, 'bezierToStart', 'to')"
      stroke="black"
      stroke-width="1"
      fill="transparent"
    />
    <!-- точки для хелпера 1 -->
    <circle
      :cx="bezierFrom.x"
      :cy="bezierFrom.y"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('bezierFrom')"
      r="5"
      class="grabable"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
    <circle
      :cx="bezierFromStart.x"
      :cy="bezierFromStart.y"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('bezierFromStart', true)"
      r="10"
      stroke="transparent"
      stroke-width="3"
      class="grabable"
      fill="rgba(0,0,0, 0.2)"
    />
    <path
      :d="`M ${bezierFromStart.x} ${bezierFromStart.y}  C ${bezierFrom.x} ${bezierFrom.y}, ${bezierTo.x} ${bezierTo.y}, ${bezierToStart.x}  ${bezierToStart.y} `"
      marker-end="url(#someMarker)"
      marker-start="url(#someMarker)"
      stroke="black"
      :id="`path${pathId}`"
      fill="transparent"
    />
    <!-- center circle 
    <circle :cx="center.x" :cy="center.y" r="30" stroke="black" stroke-width="3" fill="red" />
    -->

    <foreignObject v-if="text" :x="center.x - 40" :y="center.y-20" width="80" height="40">
      <div xmlns="http://www.w3.org/1999/xhtml" class="centered_text">{{text}}</div>
    </foreignObject>

    <!-- точки для хелпера2 -->

    <circle
      :cx="bezierTo.x"
      :cy="bezierTo.y"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('bezierTo')"
      r="5"
      stroke="black"
      stroke-width="3"
      class="grabable"
      fill="red"
    />
    <circle
      :cx="bezierToStart.x"
      :cy="bezierToStart.y"
      @dragstart="dragstartFalse"
      @mousedown="dragElement('bezierToStart', true, '', 'to')"
      r="10"
      stroke="transparent"
      stroke-width="3"
      fill="rgba(0,0,0, 0.2)"
      class="grabable"
    />
  </g>
</template>

<script>
export default {
  name: "EditablePathLine",
  props: {
    position: Object,
    text: String,
    pathId: Number,
    radius: Number
  },
  data: function() {
    return {
      r: this.radius,
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
      },
      bezierFromStart: {
        x: this.position.x1,
        y: this.position.y1 + this.radius
      },
      bezierToStart: {
        x: this.position.x2,
        y: this.position.y2 - this.radius
      },
      center: {
        x: 0,
        y: 0
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
    this.center = this.BezierCubicXY(
      this.from,
      this.bezierFrom,
      this.bezierTo,
      this.to,
      0.5
    );
  },
  methods: {
    onDraggingArrow(evt, circleX, circleY) {
      // Get the mouse position relative to the centre of the circle (circleX,circleY)
      // let ball = document.getElementById("circle1");
      // console.log(evt.pageX);
      let dx = evt.pageX - circleX;
      let dy = evt.pageY - circleY;
      // Calculate distance from centre of circle to mouse (Pythagoras' theorem)
      let distance = Math.sqrt(dx * dx + dy * dy);
      // Test against radius
      if (distance !== this.r) {
        // Scale the dx,dy coords back so they are on the circumference
        dx = (dx * this.r) / distance;
        dy = (dy * this.r) / distance;
      }

      return {
        x: dx + circleX,
        y: dy + circleY
      };
    },
    dragElement(changer, dragArrow = false, relObj = "", dotPos = "from") {
      let ball = event.target;
      let radius = ball.getAttribute("r");
      let svg = document.getElementById("test");
      ball.style.cursor = "grabbing";
      let shiftX = event.clientX - ball.getBoundingClientRect().left - radius;
      let shiftY = event.clientY - ball.getBoundingClientRect().top - radius;
      const reducedThis = this;

      svg.append(ball);

      moveAt(event.pageX, event.pageY, reducedThis, changer);

      // moves the ball at (pageX, pageY) coordinates
      // taking initial shifts into account
      function moveAt(pageX, pageY, reducedThis, changer) {
        if (dragArrow) {
          reducedThis[changer] = reducedThis.onDraggingArrow(
            event,
            reducedThis[dotPos].x,
            reducedThis[dotPos].y
          );
        } else {
          reducedThis[changer] = {
            x: parseFloat(pageX - shiftX),
            y: parseFloat(pageY - shiftY)
          };
          reducedThis.center = reducedThis.BezierCubicXY(
            reducedThis.from,
            reducedThis.bezierFrom,
            reducedThis.bezierTo,
            reducedThis.to,
            0.5
          );
          if (relObj) {
            reducedThis[relObj] = reducedThis.onDraggingArrow(
              event,
              reducedThis[dotPos].x,
              reducedThis[dotPos].y
            );
          }
        }
      }

      function onMouseMove(event) {
        moveAt(event.pageX, event.pageY, reducedThis, changer);
      }

      // move the ball on mousemove
      document.addEventListener("mousemove", onMouseMove);

      // drop the ball, remove unneeded handlers
      document.onmouseup = function() {
        document.removeEventListener("mousemove", onMouseMove);
        ball.onmouseup = null;
        ball.style.cursor = "";
      };
    },
    dragstartFalse() {
      return false;
    },
    // Points are objects with x and y properties
    // p0: start point
    // p1: handle of start point
    // p2: handle of end point
    // p3: end point
    // t: progression along curve 0..1
    // returns an object containing x and y values for the given t
    BezierCubicXY(p0, p1, p2, p3, t) {
      var ret = {};
      var coords = ["x", "y"];
      var i, k;

      for (i in coords) {
        k = coords[i];
        ret[k] =
          Math.pow(1 - t, 3) * p0[k] +
          3 * Math.pow(1 - t, 2) * t * p1[k] +
          3 * (1 - t) * Math.pow(t, 2) * p2[k] +
          Math.pow(t, 3) * p3[k];
      }

      return ret;
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
.grabable {
  cursor: grab;
}
.centered_text {
  color: #222;
  text-align: center;
  font: 14px serif;
  height: 100%;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid #222;
  border-radius: 10px;
  box-sizing: border-box;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
}
</style>
<style>
.cursor-grabbing {
  cursor: grabbing !important;
}
body {
  background: yellow;
}
</style>
