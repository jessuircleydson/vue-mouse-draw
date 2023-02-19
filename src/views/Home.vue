<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <input type="color" v-model="canvasColor">
    <canvas id="myCanvas" width="560" height="360" @mousemove="draw" @mousedown="beginDrawing" @mouseup="stopDrawing" />
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      canvas: null,
      x: 0,
      y: 0,
      isDrawing: false,
      canvasColor: 'black',
    }
  },
  methods: {
    drawLine(x1, y1, x2, y2) {
      let ctx = this.canvas;
      ctx.beginPath();
      ctx.strokeStyle = this.canvasColor;
      ctx.lineWidth = 1;
      ctx.moveTo(x1, y1);
      ctx.lineTo(x2, y2);
      ctx.stroke();
      ctx.closePath();
    },
    draw(e) {
      if(this.isDrawing) {
        this.drawLine(this.x, this.y, Math.round(e.offsetX), Math.round(e.offsetY));
        this.x = Math.round(e.offsetX);
        this.y = Math.round(e.offsetY);
      }
    },
    beginDrawing(e) {
      this.x = Math.round(e.offsetX);
      this.y = Math.round(e.offsetY);
      this.isDrawing = true;
    },
    stopDrawing(e) {
      if (this.isDrawing) {
        this.drawLine(this.x, this.y, Math.round(e.offsetX), Math.round(e.offsetY));
        this.x = 0;
        this.y = 0;
        this.isDrawing = false;
      }
    }
  },
  mounted() {
    var c = document.querySelector("#myCanvas");
    var ctx = c.getContext("2d");    
    this.canvas = ctx;
  },
}
</script>

<style lang="css" scoped>
#myCanvas {
  border: 2px solid black;
}
</style>
