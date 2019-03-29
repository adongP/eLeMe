<template>
  <div class="header">
    <div class="contentW">
      <div class="avatar">
        <img style="width:64px; height:64px" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <h3 class="title"><span class="brand"></span> <span class="name">{{seller.name}}</span></h3>
        <p class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</p>
        <div class="supports" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span style="font-size:10px;line-height:12px">{{seller.supports[0].description}}</span>
        </div>
        <div @click="showDetail" v-if="seller.supports" style="position:absolute; right:0;bottom:0;font-size:10px;line-height:12px;padding:7px;background:rgba(0,0,0,0.2);border-radius: 14px;">
          <span>{{seller.supports.length}}个
            <i class="icon-keyboard_arrow_right"></i> 
          </span>
      </div>
      </div>
      
    </div>
    <div class="noticeW">
      <div @click="showDetail">
        <span class="bulletin"></span><span style="display:inline-block;width: 85%; font-size:10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap;">{{seller.bulletin}}</span>
        <span><i class="icon-keyboard_arrow_right"></i> </span>
        </div>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="">
    </div>
    <div v-show="detailShow" class="detail" transition="fade">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h3 class="name"><span>{{seller.name}}</span></h3>
          <p style="text-align:center;padding:16px 0px 28px;"><star :size="48" :score="seller.score"></star></p>
          <div class="title">
            <div class="line"></div>
            <div class="text">
              优惠信息
            </div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="support">
            <li class="support-item" v-for="item in seller.supports" track-by="$index">
              <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
              <span>{{seller.supports[$index].description}}</span>
            </li>
          </ul>
           <div class="title">
            <div class="line"></div>
            <div class="text">
              商家公告
            </div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
              {{seller.bulletin}}
          </div>
        </div>
      </div>
      <div class="tetail-close">
        <i class="icon-close" @click="closeDEtail"></i>
      </div>
      <div class="blur" style="width:100%;height:100%;pisition:fixed;z-index:-1;background:red"></div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import star from 'components/star/star';
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      detailShow: false
    };
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    closeDEtail() {
      this.detailShow = false;
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components: {
    star
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
 @import "../../common/stylus/mixin.styl";
.header
  color :#fff
  position :relative
  overflow :hidden
  background:rgba(7,17,27,0.4)
  .contentW
    display :flex
    align-items :center
    padding :24px 12px 18px 24px
    .content
      flex:1
      position:relative
      margin-left 16px
      .title
        margin :2px 0px 8px 0px
        .brand
          display :inline-block
          vertical-align :top
          width :30px
          height :18px
          bg-image('brand')
          background-size :30px 18px
          background-repeat :no-repeat
        .name
          line-height :18px
          font-size :16px
          font-weight :bold
          margin-left :8px
      .description
        line-height :12px
        font-size :12px
        margin-bottom :10px
      .supports
        line-height 12px
        .icon
          display :inline-block
          width:12px
          height :12px
          margin-right:4px
          background-size:12px 12px
          background-repeat:no-repeat
          &.decrease
            bg-image('decrease_1')
          &.discount 
            bg-image('discount_1')
          &.guarantee 
            bg-image('guarantee_1')
          &.invoice 
            bg-image('invoice_1')
          &.special 
            bg-image('special_1')
  .noticeW
    padding:7px 4px 7px 12px
    background:rgba(7,17,27,0.2)
    .bulletin
      display:inline-block
      width:24px
      height:12px
      margin-right :4px
      vertical-align :middle
      bg-image("bulletin")
      background-size:24px 12px
      background-repeat:no-repeat
  .background 
    position :absolute
    top:40px
    left:0
    width:100%
    height:100%
    z-index :-1
    filter:blur(10px)
    img
      width:70%
      display:block
      margin:0 auto
  .detail
    position fixed
    top 0
    left 0
    z-index 100
    width:100%
    height 100%
    overflow auto 
    background rgba(7,17,27,0.8)
    backdrop-filter:blur(10px)
    // filter blur(10px)
    transition:all 0.5s
    &.fade-transition
      opactiy:1
      backgound:rgba(7,17,27,0.8)
    &.fade-enter,&.fade-leave
      opacity:0
      background:rgba(7,17,27,0)
    .detail-wrapper
      min-height:100%
      width:100%
      .detail-main
        margin-top:64px
        padding-bottom 64px
        .name
          font size 16px
          font-weight:700
          line-height 16px
          text-align:center
        .title
          display flex 
          align-items: center
          margin:0 36px
          .line
            flex:1
            border-bottom:1px solid rgba(255,255,255,0.2)
            height:0px
          .text
            font size 14px
            font-weight:700
            list-height 14px
            padding:0px 12px
        .support
           margin:24px 48px 28px
           .support-item
              margin-bottom 12px
              font-size 12px
              font-weight 200
              line-height 12px
              display: flex;
              align-items: center;
          .icon
            display:inline-block
            width:16px
            height 16px
            margin-right:6px
            background-size:16px 16px
            background-repeat:no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount 
              bg-image('discount_1')
            &.guarantee 
              bg-image('guarantee_1')
            &.invoice 
              bg-image('invoice_1')
            &.special 
              bg-image('special_1')
        .bulletin
            margin: 24px 48px 28px
            font-size 12px
            font-weight:200
            line-height:24px
    .tetail-close{
      position relative
      width:32px
      height:32px
      margin:-64px auto 0 auto
      clear: both;
      font-size:32px
    }
</style>
