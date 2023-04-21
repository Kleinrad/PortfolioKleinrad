<template>
    <canvas id="drip_canvas"></canvas>
</template>

<script>
class Drip{
    constructor(ctx, x, y, radius) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.radius = radius;
        this.speed = 2;
        this.delay = Math.random() * 100+20;
        this.positive = true;
        this.done = false;
        this.particleSpawned = false;
        this.state = 0;
    }

    draw() {
        if (this.delay > 0 || this.done) {
            this.delay -= 1;
            return;
        }
        let ex_delta = Math.sin((this.state*180) /180 * Math.PI) * this.radius;
        let ey_delta = this.state * this.radius*2;
        
        let sx_delta = this.radius*2;

        let cx_delta = this.radius*2 * (1-this.state);
        let cy_delta = this.state * this.radius;

        let arcY_delta = this.state * this.radius*4 - this.radius*1.1;

        if(this.positive){
            this.ctx.beginPath();
            this.ctx.arc(this.x, this.y + arcY_delta, this.radius, Math.PI/2 + -Math.PI*this.state*1.5, Math.PI/2 +Math.PI*this.state*1.5, false);
            this.ctx.fill();

            this.ctx.beginPath();
            this.ctx.moveTo(this.x - sx_delta, this.y);
            this.ctx.quadraticCurveTo(this.x - cx_delta, this.y + cy_delta, this.x-ex_delta, this.y + ey_delta);
            this.ctx.lineTo(this.x+ex_delta, this.y + ey_delta);
            this.ctx.quadraticCurveTo(this.x + cx_delta, this.y + cy_delta, this.x + sx_delta, this.y);
            this.ctx.lineTo(this.x+sx_delta, this.y);
        }else{
            this.ctx.beginPath();
            this.ctx.moveTo(this.x - sx_delta, this.y);
            this.ctx.quadraticCurveTo(this.x, this.y + Math.max(0,arcY_delta), this.x+sx_delta, this.y);
        }

        this.ctx.closePath();

        this.ctx.fillStyle = "rgba(255, 255, 255, 1)";
        this.ctx.lineWidth = 2;
        this.ctx.fill();

        
        if (this.state == 1) {
            this.positive = false;
        }
        this.state = Math.min(1,Math.max(0,this.state + (this.positive ? 0.01*this.speed : -0.035*this.speed)));
        if (this.state == 0) {
            this.done = true;
        }
    }

    getPointLocation() {
        return {x:this.x, y:this.y + (this.radius*4-this.radius*1.1)}
    }
}

export default {
    data () {
        return {
            ctx: null,
            canvas: null,
            drips: []
        }
    },
    props: {
        drip: Boolean
    },
    emits: ["newParticle"],
    methods: {
        animate(){
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
            this.drips.forEach(drip => {
                drip.draw();
                if (!drip.positive && !drip.particleSpawned) {
                    this.newParticle(drip.getPointLocation());
                    drip.particleSpawned = true;
                }else if(drip.done){
                    this.drips.splice(this.drips.indexOf(drip), 1);
                    this.drips.push(new Drip(this.ctx, (Math.random()*16-8)+this.canvas.width*0.08, 0, 3+(Math.random()*2-1)));
                }
            });
            if (this.drip){
                requestAnimationFrame(this.animate);
            }
        },
        newParticle(pos) {
            this.$emit("newParticle", pos);
        }
    },
    mounted() {
        this.canvas = document.getElementById("drip_canvas");
        this.ctx = this.canvas.getContext("2d");

        this.canvas.width = this.$el.offsetWidth;
        this.canvas.height = this.$el.offsetHeight;

        for (let i = 0; i < 50; i++) {
            this.drips.push(new Drip(this.ctx, (Math.random()*16-8)+this.canvas.width*0.08, 0, 3+(Math.random()*2-1)));
        }

        this.animate();
    }
}
</script>

<style scoped>
#drip_canvas {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    width: 100%;
    height: 50%;
    pointer-events: none;
}
</style>