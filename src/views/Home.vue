<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" id="logo" />
    <div class="canvas-container">
      <div class="control-group">

        <input type="color" v-model="canvasColor">
         <div class="input-group">
           <label for="lineWidth">Largura da Linha</label>
           <input id="lineWidth" type="number" v-model="canvasLineWidth">
         </div>
      </div>
      <canvas id="myCanvas" :width="canvasW" :height="canvasH" @mousemove="draw" @mousedown="beginDrawing" @mouseup="stopDrawing" />
    </div>
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
      canvasW: 0,
      canvasH: 0,
      canvasLineWidth: 1,
    }
  },
  methods: {
    drawLine(x1, y1, x2, y2) {
      let ctx = this.canvas;
      ctx.beginPath();
      ctx.strokeStyle = this.canvasColor;
      ctx.lineWidth = this.canvasLineWidth;
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
    this.canvasW = window.innerWidth - 100;
    this.canvasH = window.innerHeight - 200;
    var ctx = c.getContext("2d");
    this.canvas = ctx;
  },
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#logo {
    max-width: 50px;
}
.canvas-container {
  padding: 20px;
  .control-group{
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    gap: 20px;

    .input-group {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
    }
  }
}
#myCanvas {
  border: 2px solid black;
}
</style>
