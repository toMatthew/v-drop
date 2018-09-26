<template>
<div class="drop-box">
    <div class="drop-contanier" ref="contanier">
        <div class="drop-left" ref="left"></div>
        <div class="drop-btn" ref="btn" @touchstart ="touchstart"></div>
        <div class="drop-right" ref="right"></div>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            isClick : false,
            left: 0,//当前坐标
            maxPistion : 0,//最大坐标
            money : 5000,//当前金额
        }
    },
    computed :{
        diffValue () {//最大和最小差值
            return this.max - this.min;
        }
    },
    props: {
        min : {//最小值
            type : Number,
            default : 1000,
        },
        max : {//最大值
            type : Number,
            default : 10000,
            // validator:(value) => {//自定义验证函数
            // console.log(value, this.min)
            //     return value > 0;
            // },
        },
        input : {//当前金额
            type : Number,
            default : 5000,
        },
        speed : {//移动块级
            type : Number,
            default : 100,
        },
    },
    methods :{
        // 包围盒的信息坐标轴
        windowToBox(x, y, el) {
            let _bbox = this.positionObj;
            return {
                x: x - _bbox.left ,
                y: y - _bbox.top
            }
        },
        touchstart(e) {
            e.preventDefault();//阻止默认事件，取消文字选中
            this.isClick = true;
            let _point = this.windowToBox(e.targetTouches[0].pageX , e.targetTouches[0].pageY, this.$refs.btn);//得出来的是相对这垂直btn的点位置
            this.left = Math.round(_point.x);
            window.requestAnimationFrame(this.animation.bind(this))
        },
        touchmove(e) {
            if(this.isClick) {
                e.preventDefault();//阻止默认事件，取消文字选中
                let _point = this.windowToBox(e.targetTouches[0].pageX , e.targetTouches[0].pageY, this.$refs.btn);//得出来的是相对这垂直btn的点位置
                this.left = Math.round(_point.x);
                window.requestAnimationFrame(this.animation.bind(this))
            }
        },
        touchend(e) {
            this.isClick = false;
        },
        animation() {
            if(this.left > this.maxPistion) {
                this.left = this.maxPistion;
            }
            if(this.left < 0) {
                this.left = 0;
            }
            // 上面的校验很重要,上面的边界值取决于是否拿得到
            this.$refs.btn.style.transform = `translateX(${this.left}px)`;
            this.setMoney(this.left);
            this.$refs.left.style.width =  this.left + 'px';     
        },
        setMoney(left) {
            let m = 0
            m = Math.floor(((left * this.diffValue) / this.maxPistion )/ this.speed) * this.speed;
            this.money = m + this.min;
            this.$emit('input', this.money)
        },
        setLeft() {
            this.money = this.value;
            this.left = parseInt((this.maxPistion * (this.value - this.min)) / this.diffValue);
            this.$refs.btn.style.transform = `translateX(${this.left}px)`;
            this.$refs.left.style.width =  this.left + 'px';
        }
    },
    mounted() {
        this.$nextTick(()=>{
            setTimeout(()=>{
                // add 监听
                document.addEventListener('touchmove', this.touchmove, { passive: false });
                document.addEventListener('touchend', this.touchend, { passive: false });
                // 按钮的相对位置
                this.positionObj = this.$refs.btn.getBoundingClientRect();
                // 获得最大滑动的长度
                this.maxPistion = this.$refs.contanier.clientWidth;
                this.setLeft();
            },60);
        });
    },
    destroyed() {
        document.removeEventListener('touchmove', this.touchmove, { passive: false });
        document.removeEventListener('touchend', this.touchend, { passive: false });
    },
}
</script>

<style scoped>
.drop-box{
    width: 100%;
    padding:0;
    height:50px;
    margin: 0 auto;
}
.drop-contanier{
    height: 100%;
    position: relative;
    
}
.drop-left{
    height:10px;
    background: linear-gradient(left, yellow, red);
    background: -webkit-gradient(linear,left top,right top,from(#ff0),to(red));
    position: absolute;
    top:50%;
    left:0;
    transform : translateY(-50%);
    z-index: 550;
    border-radius: 5px;
}
.drop-btn{
    width: 30px;
    height:30px;
    background-color: #E53030;
    border-radius: 100%;
    position: absolute;
    top:50%;
    margin-top: -15px;
    left: -15px;
    z-index: 600;
    box-shadow: 0px 0px 8px yellow;
}
.drop-right{
    width:100%;
    height:10px;
    background-color: #E5E5E5;
    position: absolute;
    top:50%;
    transform : translateY(-50%);
    z-index: 500;
    left: 0;
    border-radius: 5px;
}
</style>
