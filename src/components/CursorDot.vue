<template>
    <div :style="{top: cursor.y + 'px', left: cursor.x + 'px', width: this.width + 'vw', height: this.height + 'vw'}" id="cursor_dot"></div>
</template>

<script>
export default {
  name: "CursorDot",
  data() {
    return {
      cursor: {
        x: 0,
        y: 0,
      },
      width: 0,
      height: 0,
      base_width: 1,
      base_height: 1,
      squash_factor: 0.2,
    };
  },
  mounted() {
    document.addEventListener("mousemove", this.onMouseMove);
  },
  methods: {
    onMouseMove(e) {
      //smooth cursor
      this.cursor.x = this.cursor.x + (e.clientX - this.cursor.x) / 2;
      this.cursor.y = this.cursor.y + (e.clientY - this.cursor.y) / 2;
      //calculate cursor size
      this.width = this.base_width;
      this.height = this.base_width;
      const { width, height } = this.squashDot(this.width, this.height, this.cursor, {
        x: e.clientX,
        y: e.clientY,
      });
      this.width = width;
      this.height = height;
    },
    squashDot(width, height, pos1, pos2) {
      const xDiff = Math.abs(pos1.x - pos2.x);
      const yDiff = Math.abs(pos1.y - pos2.y);
      const distance = Math.sqrt(xDiff ** 2 + yDiff ** 2);
      if (distance < 3) {
        return { width: this.base_width, height: this.base_width };
      }

      const x_ratio = xDiff / distance;
      const y_ratio = yDiff / distance;
      
      const x_factor = (x_ratio * this.squash_factor + 1);
      const y_factor = (y_ratio * this.squash_factor + 1);

      if (x_ratio > y_ratio) {
        const newWidth = width * x_factor;
        const newHeight = height / x_factor;
        return { width: newWidth, height: newHeight };
      }else{
        const newWidth = width / y_factor;
        const newHeight = height * y_factor;
        return { width: newWidth, height: newHeight };
      }
    }
  },
};
</script>

<style scoped>
#cursor_dot{
  position: absolute;
  background-color: #000;
  pointer-events: none;
  border-radius: 50%;
  z-index: 1000;
  transform: translate(-50%, -50%);
  mix-blend-mode: difference;
}
</style>