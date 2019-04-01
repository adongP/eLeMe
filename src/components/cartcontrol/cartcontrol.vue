<template>
    <div class="cartcontrol">
        <div class="cart-decrease" @click="decreassCount" v-show="food.count>0" transition="move">
            <span class="inner icon-remove_circle_outline" v-show="food.count>0"></span>
        </div>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click="addCount"></div>
        
    </div>
</template>

<script>
import Vue from 'vue';
export default {
    props: {
        food: {
            type: Object
        }
    },
    created() {
        // console.log(this.food);
    },
    methods: {
        addCount(event) {
            if (!event._constructed) {
                return;
            };
            if (!this.food.count) {
                Vue.set(this.food, 'count', 1);
                this.food.count = 1;
            } else {
                this.food.count ++;
            }
            this.$dispatch('cart.add', event.target);
        },
        decreassCount(event) {
            if (!event._constructed) {
                return;
            }
            if (this.food.count) {
                this.food.count --;
            }
        }
    }
};
</script>

<style lang="stylus">
    .cartcontrol
        font-size:0
        .cart-decrease
            display:inline-block;
            font-size:24px
            padding:6px
            transition: all 0.4s linear 
            &.move-transition  
                opacity:1
                transform:translate3d(0,0,0)
                .inner
                    transition: all 0.4s linear 
                    transform:rotate(0)
            &.move-enter,
            &.move-leave
                opacity:0
                transform:translate3d(44px,0,0)
                .inner 
                    transform:rotate(360deg)
            .inner
                display:inline-block
                line-height 24px
                color:rgb(0,160,220)
        .cart-count
            display:inline-block;
            vertical-align:top;
            width:12px
            padding 6px
            line-height 24px;
            text-align center
            font-size:12px
            color:rgb(147,153,159)
        .cart-add
            display:inline-block;
            padding:6px
            color:rgb(0,160,220)
            font-size:24px
</style>
