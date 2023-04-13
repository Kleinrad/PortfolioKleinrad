<template>
  <div :class="{'cursor_wrapper': true, 'wrapper_pointer': pointer}">
    <div 
        :style="{top: cursor.y + 'px', left: cursor.x + 'px', width: this.width + 'vw', height: this.height + 'vw'}"
        :class="{'cursor-pointer': pointer}"
        id="cursor_dot"
      ></div>
  </div>
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
      pointer: false,
    };
  },
  props: {
    'dynamic' : Boolean,
    'pos': Object,
  },
  mounted() {
    document.addEventListener("mousemove", this.onMouseMove);
    document.addEventListener("mouseover", this.onMouseover);
  },
  methods: {
    onMouseMove(e) {
      //smooth cursor
      this.cursor.x = this.dynamic ? this.cursor.x + (e.clientX - this.cursor.x) / 2 : this.pos.x;
      this.cursor.y = this.dynamic ? this.cursor.y + (e.clientY - this.cursor.y) / 2 : this.pos.y;
      //calculate cursor size
      this.width = this.base_width;
      this.height = this.base_width;
      if (this.dynamic){
        const { width, height } = this.squashDot(this.width, this.height, this.cursor, {
          x: e.clientX,
          y: e.clientY,
        });
        
        this.width = width;
        this.height = height;
      }
    },
    onMouseover(event) {
      const cursorType = getComputedStyle(event.target).cursor;

      if (cursorType === 'pointer') {
        this.pointer = true;
      } else {
        this.pointer = false;
    }
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
.cursor_wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 100;
  background-color: transparent;
  pointer-events: none;
  mix-blend-mode: difference;
}

.wrapper_pointer {
  mix-blend-mode: multiply !important;
}

#cursor_dot{
  position: absolute;
  background-color: #edf6f4;
  pointer-events: none;
  cursor: pointer;
  border-radius: 50%;
  z-index: 100;
  backface-visibility: hidden;
  transform: translate(-50%, -50%);
  transition: transform 0.3s ease-in-out, background-color 0.1s ease-in-out;
}

.cursor-pointer {
    background-color: var(--accent-color) !important;
    transform: scale(2.2) translate(-22.5%, -22.5%) !important;
}

</style>