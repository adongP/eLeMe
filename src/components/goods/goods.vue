<template>
    <div class="goods">
        <div class="menuW" v-el:menu-wrapper>
            <ul class="content">
                <li v-for="good in goods" :class="{'current':currentIndex===$index}" @click="selectMenu($index,$event)">
                    <p><span class="icon" v-show="classMap[goods[$index].type]" :class="classMap[goods[$index].type]"></span>{{good.name}}</p>
                </li>
            </ul>
        </div>
        <div class="foodsW" v-el:food-wrapper>
            <div class="content">
                <div v-for="good in goods" class="food-list-hook">
                    <div class="title">
                        {{good.name}}
                    </div>
                    <ul>
                        <li v-for="food in good.foods" class="foods border-1px">
                        <div class="avatar">
                            <img :src="food.image" alt="">
                        </div>
                        <div class="foodinf">
                            <h3 class="name">{{food.name}}</h3>
                            <p class="description">{{food.description}}</p>
                            <p class="description"><span>月售 {{food.sellCount}}</span><span>好评率 {{food.rating}}%</span></p>
                            <p class="price">￥<span class="newprice">{{food.price}}</span> <span class="oldPrice" v-if="food.oldPrice">￥{{food.oldPrice}}</span></p>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </div>
                        </li>
                    </ul>
                
                </div>
            </div>
        </div>
        <shopcart v-ref:shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    </div>
</template>

<script>
import BScroll from 'better-scroll';
import shopcart from 'components/shopcart/shopcart';
import cartcontrol from 'components/cartcontrol/cartcontrol';
const ERR_OK = 0;
export default {
    props: {
        seller: {
            type: Object
        }
    },
    data() {
        return {
            goods: {},
            listHeight: [],
            scrollY: 0
        };
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                let heightFirst = this.listHeight[i];
                let heightNext = this.listHeight[i + 1];
                if (!heightNext || this.scrollY >= heightFirst && this.scrollY < heightNext) {
                    return i;
                }
            }
            return 0;
        },
        selectFoods() {
            var foods = [];
            this.goods.forEach(good => {
                good.foods.forEach((food) => {
                    if (food.count) {
                        foods.push(food);
                    }
                });
            });
            return foods;
        }
    },
    created() {
        this.$http.get('/api/goods').then(res => {
            console.log(res.body);
            res = res.body;
            if (res.errno === ERR_OK) {
                this.goods = res.data;
                console.log(this.goods);
                this.$nextTick(() => {
                    this._initscroll();
                    this._calculateHeight();
                });
            }
            console.log(this.goods);
        });
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
        _initscroll() {
            this.menuScroll = new BScroll(this.$els.menuWrapper, {
                click: true
            });
            this.foodScroll = new BScroll(this.$els.foodWrapper, {
                click: true,
                probeType: 3  // 检测事实检测
            });
            this.foodScroll.on('scroll', (pos) => {
                this.scrollY = Math.abs(Math.round(pos.y));
            });
        },
        _calculateHeight() {
            // 获取 footW 的 高度
            let foodList = this.$els.foodWrapper.getElementsByClassName('food-list-hook'); // 获取元素
            let height = 0;
            this.listHeight.push(height);
            for (let i = 0; i < foodList.length; i++) {
                let item = foodList[i];
                height += item.clientHeight;
                this.listHeight.push(height);
            }
        },
        selectMenu(index, event) {
            console.log(event);
            if (event._consturcted) {
                return;
            };
            let foodList = this.$els.foodWrapper.getElementsByClassName('food-list-hook');
            console.log(foodList);
            let el = foodList[index];
            this.foodScroll.scrollToElement(el, 300);
            console.log(el);
        },
        _drop(target) {
            this.$refs.shopcart.drop(target);
        }
    },
    components: {
        shopcart,
        cartcontrol
    },
    events: {
        'cart.add'(target) {
            this._drop(target);
        }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin.styl"
    .goods
        display:flex
        position:absolute
        top:180px 
        bottom:46px
        left 0px
        right:0px
        overflow hidden
        width:100%;
        // height:100%;
        font-weight:500
        background #f3f5f7
        .menuW
            width:80px
            li
                width: 56px;
                font-size:12px 
                color:rgb(7,17,27)
                height 54px
                padding: 0 12px;
                text-align:center
                display:table
                &.current
                   position relative
                   z-index:10
                   margin-top:-1px
                   background:#fff;
                   font-weight:700
                   p
                      border-no()
                p
                    display: table-cell;
                    vertical-align: middle;
                    border-1px(rgba(7,17,27,0.1))
                    .icon
                        display:inline-block
                        width:16px
                        height 16px
                        margin-right:6px
                        background-size:16px 16px
                        background-repeat:no-repeat
                        &.decrease
                            bg-image('decrease_3')
                        &.discount 
                            bg-image('discount_3')
                        &.guarantee 
                            bg-image('guarantee_3')
                        &.invoice 
                            bg-image('invoice_3')
                        &.special 
                            bg-image('special_3')
        .foodsW
            flex:1
            overflow hidden
            .title
                height 26px
                line-height 26px
                font-size:12px 
                color:rgb(147,153,159)
                border-left:2px solid rgba(7,17,27,0.2)
                padding-left:14px
            ul
                padding: 0px 12px;
                background: #fff;
            .foods
                display:flex
                align-items:center
                border-1px(rgba(7,17,27,0.1))
                padding: 16px 0px;
                background: #fff;
                &:last-child
                    border-no()
            .avatar
                width:66px
                height:66px
                img 
                    width:100%
                    height:100%
            .foodinf
                margin:4px 0px 3px 10px
                .name
                    font-size:14px 
                    line-height:14px 
                    
                .description
                    font-size:10px
                    color:rgb(147,153,159)
                    line-height:10px 
                    margin-bottom:3px
                .price
                    font-size:10px
                    color:rgb(240,20,20)
                    font-weight:normal
                    line-height:24px
                    .newprice
                        font-size:14px 
                        font-weight:700
                    .oldPrice
                        color:rgb(147,153,159)
                .cartcontrol-wrapper
                    position:absolute
                    right:0;
                    bottom:12px
                    width:auto
        .shopcart
            position: fixed;
            left: 0;
            bottom: 0;
            width: 100%;
            height: 46px;
            background: #ddd;

</style>

