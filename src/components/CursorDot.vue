<template>
  <div v-if="screenType != 0" :class="{'cursor_wrapper': dynamic, 'wrapper_pointer': pointer, 'absolute_wrapper': !dynamic}">
    <div 
        :style="{top: cursor.y + 'px', left: cursor.x + 'px', width: this.width + 'vw', height: this.height + 'vw'}"
        :class="{'cursor-pointer': pointer, 'abs_trans' : !dynamic}"
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
      preDynamic: {
        x: 0,
        y: 0,
      },
      width: 0,
      height: 0,
      base_width: 1,
      base_height: 1,
      squash_factor: 0.2,
      pointer: false,
      last_point_abs: {
        x: 0,
        y: 0,
      },

      abs_trans_x: 0,
      abs_trans_y: 0,
    };
  },
  props: {
    'dynamic' : Boolean,
    'pos': Object,
    'abs_top_offset': Number,
    'screenType': Number,
  },
  mounted() {
    document.addEventListener("mousemove", this.onMouseMove);
    document.addEventListener("mouseover", this.onMouseover);
  },
  methods: {
    onMouseMove(e) {
      this.updateCursor(e);
    },
    updateCursor(e){
      //smooth cursor
      this.cursor.x = this.dynamic ? this.cursor.x + (e.clientX - this.cursor.x) / 2 : this.last_point_abs.x;
      this.cursor.y = this.dynamic ? this.cursor.y + (e.clientY - this.cursor.y) / 2 : this.last_point_abs.y;
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
      if(!this.dynamic) return;
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
    },
  },
  watch: {
    dynamic: function (val) {
      if (!val && this.screenType != 0){
        this.preDynamic.x = this.cursor.x;
        this.preDynamic.y = this.cursor.y;
        let width = document.getElementsByClassName("cursor_wrapper")[0].offsetWidth;
        let height = document.getElementsByClassName("cursor_wrapper")[0].offsetHeight;

        let point_b = {x: this.cursor.x, y: this.cursor.y - this.abs_top_offset};
        let pos_diff = {x: this.pos.x/100*width - point_b.x, y: this.pos.y/100*height - point_b.y};
        this.abs_trans_x = pos_diff.x+"px";
        this.abs_trans_y = pos_diff.y+"px";
        this.last_point_abs = point_b;
        this.updateCursor();
      }else if (this.screenType != 0){
        this.cursor = this.preDynamic;
        this.preDynamic = {x: 0, y: 0};
      }
    },
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

.absolute_wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 100;
  background-color: transparent;
  pointer-events: none;
}

.abs_trans{
  background-color: var(--font-color) !important;
  -webkit-transform: translate(-50%, -50%) translateX(v-bind(abs_trans_x)) translateY(v-bind(abs_trans_y)) scale(1.4) !important;
      -ms-transform: translate(-50%, -50%) translateX(v-bind(abs_trans_x)) translateY(v-bind(abs_trans_y)) scale(1.4) !important;
          transform: translate(-50%, -50%) translateX(v-bind(abs_trans_x)) translateY(v-bind(abs_trans_y)) scale(1.4) !important;
  -webkit-transition: 1s cubic-bezier(.15,-0.03,.19,.83);
  -o-transition: 1s cubic-bezier(.15,-0.03,.19,.83);
  transition: 1s cubic-bezier(.15,-0.03,.19,.83);
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
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  -webkit-transform: translate(-50%, -50%);
      -ms-transform: translate(-50%, -50%);
          transform: translate(-50%, -50%);
  -webkit-transition: background-color 0.1s ease-in-out, -webkit-transform 0.3s ease-in-out;
  transition: background-color 0.1s ease-in-out, -webkit-transform 0.3s ease-in-out;
  -o-transition: transform 0.3s ease-in-out, background-color 0.1s ease-in-out;
  transition: transform 0.3s ease-in-out, background-color 0.1s ease-in-out;
  transition: transform 0.3s ease-in-out, background-color 0.1s ease-in-out, -webkit-transform 0.3s ease-in-out;
}

.cursor-pointer {
    background-color: var(--accent-color) !important;
    -webkit-transform: scale(2.2) translate(-22.5%, -22.5%) !important;
        -ms-transform: scale(2.2) translate(-22.5%, -22.5%) !important;
            transform: scale(2.2) translate(-22.5%, -22.5%) !important;
}
</style>