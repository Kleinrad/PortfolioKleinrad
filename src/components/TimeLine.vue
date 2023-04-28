<template>
    <div class="animation_wrapper">
        <Transition name="fade">
            <DripIng v-if="!CTL || !CTL.full" :drip="!CTL || !CTL.full && false" @newParticle="spawnParticle"></DripIng>
        </Transition>
        <canvas id="timeline_canvas"></canvas>
    </div>
</template>

<script>
import DripIng from "@/components/DripIng.vue";

var canvasWidth = 0;
var canvasHeight = 0;

var mouseX = 0;
var mouseY = 0;

var resolution = 1;

var sizing = 8;
var particleSize =2;
var alphaOffset = 150;

function mixColors(colorA, colorB, value) {
  const rgbaA = colorA.match(/\d+(\.\d+)?/g).map(Number);
  const rgbaB = colorB.match(/\d+(\.\d+)?/g).map(Number);

  const mixedRGBA = rgbaA.map((valA, i) => {
    const valB = rgbaB[i];
    const mixedVal = valA * (1 - value) + valB * value;
    if (i < 3) {
      return Math.round(mixedVal);
    } else {
      return Math.round(mixedVal * 100) / 100;
    }
  });

  const isRGB = mixedRGBA[3] === undefined || mixedRGBA[3] === 1;
  const mixedColor = isRGB ? `rgb(${mixedRGBA.slice(0, 3).join(", ")})` : `rgba(${mixedRGBA.join(", ")})`;

  return mixedColor;
}


class Particle {
    constructor (ctx, x, y, radius, color, speed=0.01, accelerator=10) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.lastX = x;
        this.lastY = y;
        this.radius = radius;
        this.color = color;
        this.og_color = color;
        this.baseSpeed = speed;
        this.speed= this.baseSpeed;
        this.accelerator = accelerator;
        this.density = (Math.random() * 0.7) + 0.3;
    }

    draw () {
        let dx_mouse = this.lastX - mouseX;
        let dy_mouse = this.lastY - mouseY;
        let dist_mouse = Math.sqrt(dx_mouse * dx_mouse + dy_mouse * dy_mouse);
        if (dist_mouse < 300) {
            //mix the og_color with "rgba(161, 206, 179, 1)" based on distance
            let mix = 1 - (dist_mouse / 300);
            this.color = mixColors(this.og_color, "rgba(161, 206, 179, 1)", mix);
        }else{
            this.og_color = this.color;              
        }

        this.lastX += ((this.x - this.lastX) * 0.04 * (this.density*this.accelerator))*this.speed;
        this.lastY += ((this.y - this.lastY) * 0.04 * (this.density*this.accelerator))*this.speed;
        this.speed = Math.min(this.speed*(1+(this.density*5)), 1)

        this.ctx.beginPath();
        this.ctx.moveTo(this.lastX, this.lastY);
        this.ctx.arc(this.lastX, this.lastY, this.radius, 0, Math.PI * 2, false);
        this.ctx.fillStyle = this.color;
        this.ctx.fill();
    }

    changePosition (x, y) {
        if (this.x != x || this.y != y) {
            let dx_new = x - this.lastX;
            let dy_new = y - this.lastY;
            let dx_old = this.x - this.lastX;
            let dy_old = this.y - this.lastY;

            //check if change in direction
            if (dx_new * dx_old + dy_new * dy_old < 0) {
                this.speed = this.baseSpeed;
            }
            this.x = x;
            this.y = y;
        }
    }
}

class Line {
    constructor (ctx, x, y, length, width, color, children, maxConnections, availableWidth , cw, ch, ox, oy, long=false) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.length = length;
        this.width = width;
        this.color = color;
        this.cw = cw;
        this.ch = ch;
        this.ox = ox;
        this.oy = oy;
        this.long = long;
        this.childCount = children;
        this.maxConnections = maxConnections;
        this.children = [];
        this.availableWidth = availableWidth;
        this.initChildren();
    }

    draw () {
        this.children.forEach(child => {
            this.ctx.fillStyle = "rgba(0,0,0,0.05)";
            this.ctx.fillRect(this.ox, this.oy, this.cw,this.ch);
            child.draw();
            this.drawConnection(this.x, child.y-(Math.abs(this.x-child.x)), child.x, child.y);
        });
        this.ctx.beginPath();
        this.ctx.moveTo(this.x, this.y);
        this.ctx.lineTo(this.x, this.y + this.length);
        this.ctx.strokeStyle = this.color;
        this.ctx.lineWidth = this.width;
        this.ctx.stroke();
    }

    drawConnection(x1, y1, x2, y2) {
        //draw a quadratic curve from x1,y1 to x2,y2 with control point at x1,y2
        this.ctx.beginPath();
        this.ctx.moveTo(x2, y2);
        this.ctx.strokeStyle = this.color;
        this.ctx.lineWidth = this.width;
        this.ctx.quadraticCurveTo(x1, y2, x1, y1);
        //this.ctx.lineTo(x1, y1);
        this.ctx.stroke();
    }

    initChildren () {
        //generate maxConnections children
        let side_rand = Math.random() > 0.5 ? 1 : -1;
        if (this.childCount >= this.maxConnections && this.childCount > 0) {
            for (let i = 0; i < this.maxConnections; i++) {
                let newChildCount = Math.floor(this.childCount / (this.maxConnections+1));
                
                let newX = this.x + (this.availableWidth*0.15*Math.random() + (this.availableWidth*0.8)) * (i%2==0 ? side_rand * 1: side_rand*-1);

                let newY = this.y + Math.min(this.length, this.length * Math.random()*0.5 + (this.length / 1.7));
                let newMaxConnections = Math.floor(Math.random() * this.maxConnections) + 1;
                let newLength = this.length/(this.long?1.5:2)+Math.random()*this.length/(this.long?1.5:4);
                this.children.push(new Line(this.ctx, newX, newY, newLength, this.width, this.color, newChildCount, newMaxConnections, this.availableWidth/2, this.cw, this.ch, this.ox, this.oy, this.long));
            }
        }
    }
}

class FutureTimeLine {
    constructor (ctx, x, y, width, height, screenType, color) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.dataWidth = 140;
        this.dataHeight = 150;
        this.color = color;
        if (screenType == 0){
            this.dataWidth = 60
        }
        this.screenType = screenType;
        this.particles= [];
        
        this.initParticles(this.getNewLineData());
        this.updateParticles(this.getNewLineData());
        setInterval(() => {
            this.updateParticles(this.getNewLineData());
        }, 4000);
    }
    initParticles (lineData) {
        for (let i = 0; i < lineData.data.length; i+=4) {
            if (lineData.data[i] > 40) {
                this.particles.push(new Particle(this.ctx, 0*sizing+this.x, 0*sizing+this.y, particleSize, "rgba(255,255,255,"+(1)+")"));
            }
        }
    }
    updateParticles (lineData) {
        //change the position of the particles
        let particleIndex = 0;
        let newParticles = [];
        for (let i = 0; i < lineData.data.length; i+=4) {
            if (lineData.data[i] > 20) {
                if (particleIndex >= this.particles.length){
                    if ((i/4) % this.dataWidth == 0 || Math.floor((i/4) / this.dataWidth) == 0) continue;
                    let randIndex = Math.floor(Math.random() * this.particles.length);
                    newParticles.push(new Particle(this.ctx, this.particles[randIndex].lastX, this.particles[randIndex].lastY, particleSize, "rgba(255,255,255,"+((lineData.data[i+3]-alphaOffset) / (255-alphaOffset))+")"));
                    newParticles[newParticles.length-1].changePosition(((i/4) % this.dataWidth)*sizing+this.x, (Math.floor((i/4) / this.dataWidth)*sizing+this.y));
                }else{
                    newParticles.push(this.particles[particleIndex])
                    newParticles[particleIndex].color="rgba(255,255,255,"+((lineData.data[i+3]-alphaOffset) / (255-alphaOffset))+")";
                    newParticles[particleIndex].changePosition(((i/4) % this.dataWidth)*sizing+this.x, (Math.floor((i/4) / this.dataWidth)*sizing+this.y));
                    particleIndex++;
                }
            }
        }
        this.particles = newParticles;
    }
    getNewLineData () {
        this.lines = [];
        let offsetX = this.width - this.dataWidth;
        let offsetY = 0;
        this.ctx.clearRect(offsetX, offsetY, this.dataWidth, this.dataHeight);
        this.lines.push(new Line(this.ctx, this.dataWidth/2+offsetX, offsetY, 
                        Math.floor(50)
                        , 1, "white" , 40, 3, this.dataWidth/4, 
                        this.dataWidth, this.dataHeight, offsetX, offsetY, this.screenType == 0));
        this.lines.forEach(line => {
            line.draw();
        });
        //scan canvas for particles
        let lineData = this.ctx.getImageData(offsetX, offsetY, this.dataWidth, this.dataHeight);
        //this.ctx.strokeRect(offsetX, offsetY, this.dataWidth, this.dataHeight*(this.screenType==0?2.45:1));
        this.ctx.clearRect(offsetX, offsetY, this.dataWidth, this.dataHeight*(this.screenType==0?2.45:1));
        return lineData;
    }
    draw(){   
        for (let i = 0; i < this.particles.length; i++) {
            this.particles[i].draw();
        };
    }
}

class ConnectionTimeLine {
    constructor (ctx, x, y, width, height, color, screenType) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.color = color;
        this.full = false;
        this.screenType = screenType;
        this.particles = [];
        if (true){
            while (!this.full){
                for (let i = 0; i < 10; i++) {
                    this.particles.push(new Particle(this.ctx, this.x, this.y, particleSize, this.color, 0.1));
                }
                this.updateParticles(this.getNewLineData());
            }
        }
        setTimeout(() => {
            this.updateParticles(this.getNewLineData());
        }, 10);
    }
    addParticle (pos) {
        this.particles.push(new Particle(this.ctx, pos.x, pos.y, particleSize, this.color, 0.1));
    }
    getNewLineData(){
        let dataWidth = 100;
        let dataHeight = 100;
        if (this.screenType == 1){
            dataWidth = 100;
            dataHeight = 80;
        }
        let offsetX = this.width-dataWidth;
        let offsetY = 200;
        
        let sx = offsetX + (0.085 * this.width/(dataWidth*sizing) * dataWidth);
        let sy = offsetY;

        let timeFactor = 1;
        let time_ms = new Date().getTime();

        let c1x = dataWidth/3 + offsetX;
        let c1y =  dataHeight/2 + offsetY;
        let c2x =  dataWidth/1.5 + offsetX;
        let c2y =  dataHeight/1.5 + offsetY;

        let ex = offsetX + dataWidth-29.5;
        let ey = offsetY + dataHeight-5;
        if (this.screenType == 1){
            ex = offsetX + dataWidth-41;
            c2x =  dataWidth/1.7 + offsetX;
            c2y =  dataHeight/1.7  + offsetY;
        }else if(this.screenType == 0){
            ex = offsetX + dataWidth-74;
            c2x =  dataWidth/2.5 + offsetX;
            c2y =  dataHeight/1.9  + offsetY;
        }
        
        this.ctx.beginPath();
        this.ctx.clearRect(offsetX, offsetY, dataWidth, dataHeight);
        this.ctx.moveTo(sx, sy);
        this.ctx.bezierCurveTo(c1x, c1y, c2x, c2y, ex, ey);;
        this.ctx.stroke();        
        let lineData = this.ctx.getImageData(offsetX, offsetY, dataWidth, dataHeight);
        this.ctx.clearRect(offsetX, offsetY, dataWidth, dataHeight);

        return lineData;
    }
    updateParticles(lineData){
        let particleIndex = 0;
        for (let i = lineData.data.length-3; i >= 0; i-=4) {
            if (lineData.data[i+3] > 0 && particleIndex < this.particles.length) {

                this.particles[particleIndex].changePosition(((i/4) % 100)*sizing+this.x, (Math.floor((i/4) / 100)*sizing+this.y));
                this.particles[particleIndex].color = "rgba(255,255,255,"+((255-(Math.floor((i/4) / 100)))/255)+")";
                particleIndex++;
            }
        }
        if (particleIndex < this.particles.length) {
            this.full = true;
        }
    }
    draw () {
        for (let i = 0; i < this.particles.length; i++) {
            this.particles[i].draw();
        };
    }
}

export default {
    data () {
        return {
            canvas: null,
            ctx: null,
            lines: [],
            FTL : null,
            CTL : null,
        }
    },
    props: {
        screenType: {
            type: Number,
            default: 0
        }
    },
    methods: {
        drawTimeLine(){
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height); 
            this.FTL.draw();
            this.CTL.draw();
            requestAnimationFrame(this.drawTimeLine)
        },
        spawnParticle (pos){
            this.CTL.addParticle(pos);
        },
        updateSize(){
            this.canvas.width = canvasWidth = this.$el.offsetWidth;
            this.canvas.height = canvasHeight = this.$el.offsetHeight;
            this.FTL = new FutureTimeLine(this.ctx, 0, this.canvas.height/2, this.canvas.width, this.canvas.height);
        },
        getMouseCords (e) {
            let rect = this.canvas.getBoundingClientRect();
            mouseX = e.clientX - rect.left;
            mouseY = e.clientY - rect.top;
        },
    },
    mounted () {
        this.canvas = document.getElementById('timeline_canvas');
        this.ctx = this.canvas.getContext('2d', { willReadFrequently: true });

        setTimeout(() => {
            resolution = this.screenType == 0 ? 2.5 : 1;
            sizing *= resolution;
            particleSize *= resolution;

            this.canvas.width = canvasWidth = this.$el.offsetWidth*resolution;
            this.canvas.height = canvasHeight = this.$el.offsetHeight*resolution;
            
            let ftlOffset = {x: 0, y: 0}
            if(this.screenType == 1){
                ftlOffset.y = 600*resolution;
                ftlOffset.x = -90*resolution;
            }else if (this.screenType == 2){
                ftlOffset.y = 750*resolution;
            }else{
                ftlOffset.y = 760*resolution;
                ftlOffset.x = -40*resolution;
            }
            this.FTL = new FutureTimeLine(this.ctx, ftlOffset.x, ftlOffset.y, this.canvas.width, this.canvas.height, this.screenType);
            this.CTL = new ConnectionTimeLine(this.ctx, 0, 0, this.canvas.width, this.canvas.height, "rgba(255,255,255,1)", this.screenType);

            window.addEventListener('resize', this.updateSize);
            this.canvas.addEventListener('mousemove', this.getMouseCords);

            //draw rect to test canvas size
            this.ctx.strokeStyle = "white";
            this.ctx.strokeRect(0, 0, this.canvas.width, this.canvas.height);
            this.drawTimeLine();
        }, 100);
    },
    components: {
        DripIng
    }
}
</script>

<style scoped>

.fade-enter-active, .fade-leave-active {
    -webkit-transition: opacity .5s;
    -o-transition: opacity .5s;
    transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
    opacity: 0;
}


.animation_wrapper {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
}

#timeline_canvas {
    position: relative;
    width: 100%;
    height: 100%;
    z-index: 0;
}

@media screen and (max-width: 768px) {
    
}

</style>