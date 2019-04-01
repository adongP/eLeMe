<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight':totalCount>0}">
                        <span class="icon-shopping_cart"></span>
                    </div>
                    <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight':totalPrice>0}"><span>￥{{totalPrice}}</span></div>
                <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
            </div>
            <div class="content-right">
                <div class="pay" :class="payClass">{{payDesc}}</div> 
            </div>
        </div>
        <div class="ball-container">
            <div transition="drop" class="ball" v-for="ball in balls" v-show="ball.show">
                <div class="inner inner-hook"></div>
            </div>
            
        </div>
    </div>
</template>

<script>
export default {
    props: {
        selectFoods: {
            type: Array,
            default() {
                return [
                    {
                        price: 10,
                        count: 0
                    }
                ];
            }
        },
        deliveryPrice: {
            type: Number,
            default: 0
        },
        minPrice: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            balls: [
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                }
            ],
            dropBalls: []
        };
    },
    computed: {
        totalPrice() {
            let total = 0;
            this.selectFoods.forEach((food) => {
                total += food.price * food.count;
            });
            return total;
        },
        totalCount() {
            let count = 0;
            this.selectFoods.forEach(food => {
                count += food.count;
            });
            return count;
        },
        payDesc() {
            if (this.totalPrice === 0) {
                return `￥${this.minPrice}元起送`;
            } else if (this.totalPrice < this.minPrice) {
                let diff = this.minPrice - this.totalPrice;
                return `还差￥${diff}元起送`;
            } else {
                return '去结算';
            }
        },
        payClass() {
            if (this.totalPrice < this.minPrice) {
                return 'not-enough';
            } else {
                return 'enough';
            }
        }

    },
    methods: {
        drop(el) {
            console.log(this.balls.length);
            for (let i = 0; i < this.balls.length; i++) {
                let ball = this.balls[i];
                if (!ball.show) {
                    ball.show = true;
                    ball.el = el;
                    this.dropBalls.push(ball);
                    return;
                }
            }
        },
        transitions: {
            drop: {
                beforeEnter(el) {
                    let count = this.balls.length;
                    while (count--) {
                        let ball = this.balls[count];
                        if (ball.show) {
                            let rect = ball.el.getBoundingClientRect();
                            let x = rect.left - 20;
                            let y = -(window.innerHeight - rect.top - 22);
                            el.style.display = '';
                            el.style.webkitTransform = `translate3d(0,${y}px,0)`;
                            el.style.transform = `translate3d(0,${y}px,0)`;
                            let inner = el.getElementsByClassName('inner-hook')[0];
                            inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
                            inner.style.transform = `translate3d(${x}px,0,0)`;
                        }
                    }
                },
                enter(el) {
                    /* eslint-disable no-unused-vars */
                    let rf = el.offsetHeight;
                    this.$nextTick(() => {
                        el.style.webkitTransform = 'translate3d(0,0,0)';
                        el.style.transform = 'translate3d(0,0,0)';
                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webkitTransform = 'translate3d(0,0,0)';
                        inner.style.transform = 'translate3d(0,0,0)';
                    });
                },
                afterEnter(el) {
                    let ball = this.dropBalls.shift();
                    if (ball) {
                        ball.show = false;
                        el.style.display = 'none';
                    }
                }
            }
        }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.shopcart
    position:fixed
    left:0
    bottom:0
    z-z-index:50
    width:100%;
    height:48px
    .content
        display:flex
        align-items:center
        line-height: 48px;
        color: rgba(255,255,255,0.4);
        background:#141d27
        .content-left
            flex:1
            display:flex
            .logo-wrapper
                display:flex
                width: 56px;
                height: 56px;
                background: #141d27;
                border-radius: 50%;
                position: relative;
                top: -10px;
                margin: 0px 12px 8px;
                border: 6px solid #141d27;
                box-sizing: border-box;
                .logo 
                    width: 44px;
                    height: 44px;
                    text-align: center;
                    border-radius: 50%;
                    font-size: 24px;
                    box-sizing: border-box;
                    background: rgba(255,255,255,0.3);
                    &.highlight 
                        background:rgb(0,160,220)
                        color:#fff;
                .num 
                    position: absolute;
                    top: 0;
                    right: 0;
                    width: 24px;
                    font-size: 12px;
                    height: 18px;
                    background: red;
                    border-radius: 9px;
                    color:#fff;
                    line-height: 18px;
                    text-align: center;
                    border-shadow:0 4px 8px 0 rgba(0,0,0,0.4)
            .price
                width:auto
                overflow hidden
                font-size: 16px;
                font-weight: 700;
                height: 56px;
                &.highlight
                    color:#fff
                span
                    padding: 2px 12px 2px 0px;
                    border-right: 1px solid;
            .desc
                padding-left: 12px;
                flex:1
                height: 56px;
        .content-right
            width:105px
            height: 58px;
            line-height: 48px;
            background: rgb(43,51,59);
            text-align: center;
            font-size: 12px;
            font-weight: 700;
            margin-top: -4px;
            .pay
                &.not-enough
                    background #2b333b
                &.enough
                    background #00b43c
                    color:#fff
    .ball-container
        .ball   
            position fixed
            left 20px
            bottom 22px
            z-index 200
            &.drop-transition
                transition:all 0.4s cubic-bezier(.49,-0.29,0.75,0.41)
                .inner
                    width:16px;
                    height:16px;
                    bordor-radius:50%
                    background:rgb(0,160,220)
                    transition:all 0.4s liner

</style>
