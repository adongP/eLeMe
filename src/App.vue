<template>
  <div>
    <div class="header">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab border-1px">
      <div class="tab-item">
        <a v-link="'/goods'">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="'/ratings'">评论</a>
      </div>
      <div class="tab-item">
        <a v-link="'/seller'">商家</a>
      </div>

    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import header from './components/header/header';
// import goods from './components/goods/goods';
// import ratings from './components/ratings/ratings';
// import seller from './components/seller/seller';
const ERR_OK = 0;
export default {
  data() {
    return {
      seller: {}
    };
  },
  created() {
    this.$http.get('/api/seller').then(res => {
      console.log(res.data);
      res = res.body;
      // console.log(res);
      if (res.errno === ERR_OK) {
        this.seller = res.seller;
        console.log(res.seller);
      }
    });
  },
  components: {
    'v-header': header
    // 'v-goods': goods,
    // 'v-ratings': ratings,
    // 'v-seller': seller
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import './common/stylus/mixin.styl';

.tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  font-weight: 600;
  // border-bottom:1px solid rgba(7,17,27,0.1)
  border-1px(rgba(7, 17, 27, 0.1));

  .tab-item {
    flex: 1;
    text-align-items: center;
    text-align: center;

    a {
      display: block;
      font-size: 14px;
      color: rgb(77, 85, 93);

      &.active {
        color: rgb(240, 20, 20);
      }
    }
  }
}
</style>
