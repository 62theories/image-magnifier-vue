<template>
  <div
    class="img-magnifier-container"
    @mousemove="moveMagnifier"
    @touchmove="moveMagnifier"
  >
    <div ref="glass" class="img-magnifier-glass" />
    <img
      ref="img"
      style="width: 400px; height: 400px;"
      src="https://www.humanesociety.org/sites/default/files/styles/1240x698/public/2020-07/kitten-510651.jpg?h=f54c7448&itok=ZhplzyJ9"
      @mousemove="moveMagnifier"
      @touchmove="moveMagnifier"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      zoom: 3
    };
  },
  methods: {
    magnify() {
      const img = this.$refs.img;
      const glass = this.$refs.glass;
      glass.style.backgroundImage = "url('" + img.src + "')";
      glass.style.backgroundRepeat = "no-repeat";
      glass.style.backgroundSize =
        img.width * this.zoom + "px " + img.height * this.zoom + "px";
    },
    moveMagnifier(e) {
      e.preventDefault();
      const img = this.$refs.img;
      const glass = this.$refs.glass;
      if (img && glass) {
        const bw = 3;
        const w = glass.offsetWidth / 2;
        const h = glass.offsetHeight / 2;
        const pos = this.getCursorPos(e);
        let x = pos.x;
        let y = pos.y;
        if (x > img.width - w / this.zoom) {
          x = img.width - w / this.zoom;
        }
        if (x < w / this.zoom) {
          x = w / this.zoom;
        }
        if (y > img.height - h / this.zoom) {
          y = img.height - h / this.zoom;
        }
        if (y < h / this.zoom) {
          y = h / this.zoom;
        }
        glass.style.left = x - w + "px";
        glass.style.top = y - h + "px";
        glass.style.backgroundPosition =
          "-" +
          (x * this.zoom - w + bw) +
          "px -" +
          (y * this.zoom - h + bw) +
          "px";
      }
    },
    getCursorPos(e) {
      const img = this.$refs.img;
      var a,
        x = 0,
        y = 0;
      e = e || window.event;

      /* Get the x and y positions of the image: */
      a = img.getBoundingClientRect();
      /* Calculate the cursor's x and y coordinates, relative to the image: */
      x = e.pageX - a.left;
      y = e.pageY - a.top;
      /* Consider any page scrolling: */
      x = x - window.pageXOffset;
      y = y - window.pageYOffset;
      return { x: x, y: y };
    }
  },
  mounted() {
    this.magnify();
  }
};
</script>

<style>
.img-magnifier-container {
  position: relative;
}

.img-magnifier-glass {
  position: absolute;
  border: 3px solid #000;
  border-radius: 50%;
  cursor: none;
  /*Set the size of the magnifier glass:*/
  width: 100px;
  height: 100px;
}
</style>
