<template>
    <canvas id="project_canvas"></canvas>
</template>

<script>
export default {
    data() {
        return {
            canvas: null,
            ctx: null,
            lastDst: 0,
            lastReal: {x: 0, y: 0},
            lastBr: {x: 0, y: 0},
            bezierPoints: [],
            resolution: 1,
            wigglyLines: 0,
            linesDrawn: 0,
        };
    },
    props: {
        lineLength: {
            type: Number,
            default: 86,
        },
        projectCount: {
            type: Number,
            default: 0,
        },
        lineColor: {
            type: String,
            default: "#fff",
        },
        lineWidth: {
            type: Number,
            default: 2,
        },
    },
    methods: {
        updateSize() {
            let width = Math.round(this.$el.offsetWidth);
            let height = Math.round(width * 0.45 * this.projectCount);
            this.canvas.width = width;
            this.canvas.height = height;
        },
        drawLine(dst){
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
            this.ctx.beginPath();
            this.linesDrawn = 0;
            this.ctx.strokeStyle = this.lineColor;
            this.ctx.lineWidth = this.lineWidth;
            this.bezierPoints = []
            this.lastReal = {x: 0, y: 0}
            this.lastBr = {x: 0, y: 0}
            let len_c = 0;
            let len_m = (dst/100) * (100+76*(this.projectCount-1));
            let right_b = 8 * this.resolution;
            let left_b = 84 * this.resolution;

            let v_seg = (100*this.resolution) / this.projectCount;

            this.moveTo(right_b , 0);
            for(let i = 0; i < this.projectCount; i++){
                let x = i%2 == 0 ? right_b : left_b;
                if (len_m - len_c > v_seg){
                    this.drawTo(x, v_seg * (i+1), i == this.projectCount-1);
                    len_c += Math.round(v_seg);
                }else{
                    this.drawTo(x, v_seg * i + (len_m - len_c));
                    break
                }

                if (i != this.projectCount-1 && len_m - len_c > 76){
                    this.drawTo(i%2 != 0 ? right_b : left_b, v_seg * (i+1), len_m - (len_c+76) <= v_seg);
                    len_c += 76;
                }else if (i != this.projectCount-1){
                    this.drawTo(x + (i%2==0 ? (len_m-len_c) : -(len_m-len_c)), v_seg * (i+1));
                    break
                }
            }
            this.ctx.stroke();
            for(let i = 0; i < this.bezierPoints.length-1; i++){
                let rect_x = this.bezierPoints[i][1].x;
                let rect_y = this.bezierPoints[i][1].y;
                let rect_width = this.bezierPoints[i+1][0].x-this.bezierPoints[i][1].x;
                let rect_height = this.bezierPoints[i+1][0].y-this.bezierPoints[i][1].y;

                let rect_offset = 40;

                if(i % 4 == 3){
                    rect_width -= rect_offset + (this.lineWidth/2-1);
                    rect_y -= rect_offset;
                    rect_x -= (this.lineWidth/2-1);
                    rect_height += rect_offset-(this.lineWidth/2-1);
                }else if (i % 4 == 2){
                    rect_x += rect_offset;
                    rect_y += (this.lineWidth/2-1);
                    rect_width -= rect_offset-(this.lineWidth/2-1);
                    rect_height += rect_offset + (this.lineWidth/2-1);
                }else if (i % 4 == 1){
                    rect_y -= rect_offset;
                    rect_x += (this.lineWidth/2-1);
                    rect_height += rect_offset-(this.lineWidth/2-1);
                    rect_width += rect_offset + (this.lineWidth/2-1);
                }else if (i % 4 == 0){
                    rect_x -= rect_offset;
                    rect_y += (this.lineWidth/2-1);
                    rect_width += rect_offset-(this.lineWidth/2-1);
                    rect_height += rect_offset+(this.lineWidth/2-1);
                }

                //clear rect from this.bezierPoints[i][1] to this.bezierPoints[i+1][0]
                this.ctx.clearRect(rect_x, rect_y, rect_width, rect_height);
                //quadratic curve from this.bezierPoints[i][1] to this.bezierPoints[i+1][0] with control point this.bezierPoints[i][2]
                this.ctx.beginPath();
                this.ctx.strokeStyle = this.lineColor;
                this.ctx.lineWidth = this.lineWidth;
                this.ctx.moveTo(this.bezierPoints[i][1].x, this.bezierPoints[i][1].y);
                this.ctx.quadraticCurveTo(this.bezierPoints[i][2].x, this.bezierPoints[i][2].y, this.bezierPoints[i+1][0].x, this.bezierPoints[i+1][0].y);
                this.ctx.stroke();
            }
        },
        moveTo(x, y){
            let x_rel = x / (100*this.resolution) * this.canvas.width;
            let y_rel = y / (100*this.resolution) * this.canvas.height;
            this.ctx.moveTo(x_rel, y_rel);
            this.lastReal = {x: x_rel, y: y_rel}
            this.lastBr = {x: x_rel, y: y_rel}
        },
        drawTo(x,y, forceEnd=false){
            this.bezierPoints.push(this.brezierTo(x,y, forceEnd));
            this.linesDrawn++;
        },
        lineTo(x, y){
            let x_rel = x / (100*this.resolution) * this.canvas.width;
            let y_rel = y / (100*this.resolution) * this.canvas.height;
            this.ctx.lineTo(x_rel, y_rel);
            this.ctx.moveTo(x_rel, y_rel);
        },
        brezierTo(x,y, forceEnd=false){
            let x_rel = x / (100*this.resolution) * this.canvas.width;
            let y_rel = y / (100*this.resolution) * this.canvas.height;

            let timeFactor = 3;
            //calculate random control points use time as seed
            let time_ms = new Date().getTime();
            let x1 = (((Math.cos(time_ms/(1000/timeFactor))+1) * (Math.cos(time_ms/(1500/timeFactor))+1) * (Math.cos(time_ms/(3000/timeFactor))+1))/8)*.5+.25;
            let x2 = (((Math.cos(time_ms/(800/timeFactor))+1) * (Math.cos(time_ms/(1700/timeFactor))+1) * (Math.cos(time_ms/(2700/timeFactor))+1))/8)*.5+.25;
            let y1 = (((Math.sin(time_ms/(1000/timeFactor))+1) * (Math.sin(time_ms/(1500/timeFactor))+1) * (Math.sin(time_ms/(3000/timeFactor))+1))/8)*.5+.25;
            let y2 = (((Math.sin(time_ms/(800/timeFactor))+1) * (Math.sin(time_ms/(1700/timeFactor))+1) * (Math.sin(time_ms/(2700/timeFactor))+1))/8)*.5+.25;

            let abs_para = this.normBrezier(x1, y1, x2, y2, x_rel, y_rel, 250, forceEnd);
            
            let bP1 = this.getBezierXY(0.1, this.lastBr.x, this.lastBr.y, abs_para.x1, abs_para.y1, abs_para.x2, abs_para.y2, abs_para.x_rel, abs_para.y_rel)
            let bP2 = this.getBezierXY(0.9, this.lastBr.x, this.lastBr.y, abs_para.x1, abs_para.y1, abs_para.x2, abs_para.y2, abs_para.x_rel, abs_para.y_rel)

            this.lastBr = {x: abs_para.x_rel, y: abs_para.y_rel}
            this.lastReal = {x: x_rel, y: y_rel}
            return [bP1, bP2, {x: abs_para.x_rel, y: abs_para.y_rel}];
        },
        normBrezier(x1,y1,x2,y2,x_rel,y_rel, amp, forceEnd){
            let x_diff = x_rel - this.lastReal.x;
            let y_diff = y_rel - this.lastReal.y;

            if(x_diff == 0){
                x1 = x1*amp - amp/2 + this.lastReal.x;
                x2 = x2*amp - amp/2 + this.lastReal.x;
                y1 = y1*y_diff + this.lastReal.y;
                y2 = y2*y_diff + this.lastReal.y;
            }else{
                let tmp = x1
                x1 = y1;
                y1 = tmp;
                tmp = x2
                x2 = y2;
                y2 = tmp;
                x1 = x1*x_diff + this.lastReal.x;
                x2 = x2*x_diff + this.lastReal.x;
                y1 = y1*amp - amp/2 + this.lastReal.y;
                y2 = y2*amp - amp/2 + this.lastReal.y;
            }
            if(!forceEnd){
                x_rel = x_rel - (x_rel - x2) * 0.3;
                y_rel = y_rel - (y_rel - y2) * 0.3;
            }
            this.ctx.bezierCurveTo(x1, y1, x2, y2, x_rel, y_rel);
            return {x1: x1, y1: y1, x2: x2, y2: y2, x_rel: x_rel, y_rel: y_rel}
        },
        getBezierXY(t, sx, sy, cp1x, cp1y, cp2x, cp2y, ex, ey) { //source: http://www.independent-software.com/determining-coordinates-on-a-html-canvas-bezier-curve.html
            return {
                x: Math.pow(1-t,3) * sx + 3 * t * Math.pow(1 - t, 2) * cp1x 
                + 3 * t * t * (1 - t) * cp2x + t * t * t * ex,
                y: Math.pow(1-t,3) * sy + 3 * t * Math.pow(1 - t, 2) * cp1y 
                + 3 * t * t * (1 - t) * cp2y + t * t * t * ey
            };
        }
    },
    mounted() {
        this.canvas = document.getElementById("project_canvas");
        this.ctx = this.canvas.getContext("2d");
        this.updateSize();
        window.addEventListener("resize", this.updateSize);

        setInterval(() => {
            this.lastDst = Math.min(this.lastDst + 0.4, this.lineLength);
            this.drawLine(this.lastDst);
        }, 20);
    },
}
</script>

<style>
#project_canvas {
    background-color: transparent;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

</style>