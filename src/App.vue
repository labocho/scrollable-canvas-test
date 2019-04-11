<template>
  <div id="app">
    <div :style="windowStyle" @scroll="onScroll" >
      <div :style="scrollContainerStyle">
        <canvas ref="canvas" :width="width" :height="height" :style="canvasStyle" />
      </div>
    </div>
    <label>
      Number of Circles:
      <input v-model.number="numberOfCircles" type="number">
    </label>
    <div>
      Drawn in {{ time }}ms.
    </div>
  </div>
</template>

<script>
const CANVAS_WIDTH = 400;
const CANVAS_HEIGHT = 300;
const SCROLL_CONTAINER_WIDTH = 800;
const SCROLL_CONTAINER_HEIGHT = 600;

// https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/resetTransform
if (typeof(CanvasRenderingContext2D.prototype.resetTransform) !== "function") {
  CanvasRenderingContext2D.prototype.resetTransform = function() {
    this.setTransform(1, 0, 0, 1, 0, 0);
  };
}

export default {
  computed: {
    canvasStyle() {
      return {
        height: `${CANVAS_HEIGHT}px`,
        width: `${CANVAS_WIDTH}px`,
      };
    },
    drawValues() {
      return [this.numberOfCircles, this.scrollLeft, this.scrollTop];
    },
    scrollContainerStyle() {
      return {
        background: "#888",
        height: `${SCROLL_CONTAINER_HEIGHT}px`,
        width: `${SCROLL_CONTAINER_WIDTH}px`,
      };
    },
    height() {
      return CANVAS_HEIGHT;
    },
    width() {
      return CANVAS_WIDTH;
    },
    windowStyle() {
      return {
        border: "1px solid #000",
        height: `${CANVAS_HEIGHT}px`,
        overflowX: "scroll",
        overflowY: "scroll",
        width: `${CANVAS_WIDTH}px`,
      };
    }
  },
  data: function () {
    return {
      time: 0,
      scrollLeft: 0,
      scrollTop: 0,
      numberOfCircles: 1,
    }
  },
  methods: {
    draw() {
      const t = new Date();
      const ctx = this.$refs.canvas.getContext("2d");

      ctx.resetTransform();
      ctx.fillStyle = "#fff";
      ctx.lineWidth = 1;
      ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);

      // draw fixed grid
      for(let x = 0; x < CANVAS_WIDTH; x += 100) {
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, CANVAS_HEIGHT);
        ctx.stroke();
      }

      for(let y = 0; y < CANVAS_HEIGHT; y += 100) {
        ctx.beginPath();
        ctx.moveTo(0, y);
        ctx.lineTo(CANVAS_WIDTH, y);
        ctx.stroke();
      }

      // draw circles at center
      ctx.translate(-this.scrollLeft, -this.scrollTop);

      for(let i = 1; i <= this.numberOfCircles; i++){
        ctx.beginPath();
        ctx.ellipse(
          SCROLL_CONTAINER_WIDTH / 2,
          SCROLL_CONTAINER_HEIGHT / 2,
          (i / this.numberOfCircles) * 100,
          (i / this.numberOfCircles) * 100,
          0,
          0,
          Math.PI * 2,
        );
        ctx.stroke();
      }

      this.time = new Date().getTime() - t.getTime();
    },
    onScroll(e) {
      const sx = e.currentTarget.scrollLeft;
      const sy = e.currentTarget.scrollTop;

      this.scrollLeft = sx;
      this.scrollTop = sy;

      this.$refs.canvas.style.transform = `translate(${sx}px, ${sy}px)`;
    },
  },
  mounted() {
    this.draw();
  },
  watch: {
    drawValues() {
      this.draw();
    },
  },
}
</script>

<style scoped>
</style>
