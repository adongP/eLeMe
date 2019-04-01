<template>
    <div class="ratingW">
        <div class="rating">
            <div class="rating-left">
                <h1 class="score">3.9</h1>
                <h2 class="score">綜合評分</h2>
                <p>高於周邊商家69.2%</p>
            </div>
            <div class="rating-right">
                <h2><span>服務態度</span> <star :size="36" :score="seller.score"></star></h2>
                 <h2><span>服務態度</span> <star :size="36" :score="seller.score"></star></h2>
            </div>
        </div>
    </div>
</template>

<script>
import star from 'components/star/star';
export default {
    data() {
        return {
            ratings: {}
        };
    },
    created() {
        console.log(1);
        this.$http.get('/api/ratings').then(res => {
            console.log(res);
        });
    },
    components: {
        star
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin.styl"
    .ratingW
        .rating
            display:flex
            // align-items:center 
            height 81px
            padding:18px 0px
            border-1px(rgb(145,153,159))
            .rating-left
                width:137.5px 
                text-align: center;
                h1.score
                    font-size 24px
                    color:rgb(255,153,0)
                    line-height:28px 
                    margin:18px 0px 6px 

                h2.score
                    font-size:12px 
                    color:rgb(7,17,27)
                    line-height:12px 
                    margin-bottom:8px 
                &>p 
                    font-size:10px 
                    color:rgb(145,153,159)
                    line-height 10px
                    margin-bottom:6px

</style>
