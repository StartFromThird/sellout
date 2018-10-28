<template>
  <div class="Rating" ref="rating">
    <div class="rating-wrapper">
      <div class="rating-head clearfloat">
        <div class="left">
          <div class="score">{{seller.score}}</div>
          <div class="title">综合评分</div>
          <div class="compare">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="right">
          <div class="score-wrapper clearfloat">
            <div class="title">服务态度</div>
            <show-star :size=36 :score="seller.serviceScore"></show-star>
            <div class="score">{{seller.serviceScore}}</div>
          </div>
          <div class="score-wrapper">
            <div class="title">商品评分</div>
            <show-star :size=36 :score="seller.foodScore"></show-star>
            <div class="score">{{seller.foodScore}}</div>
          </div>
          <div class="score-wrapper">
            <div class="title">送达时间</div>
            <div class="time">{{seller.deliveryTime}}分钟</div>
          </div>
        </div>
      </div>
      <div class="splitBox"></div>
      <div class="rating-select-wrapper">
        <rating-select></rating-select>
      </div>
      <div class="rating-content">
        <ul>
          <li v-for="item in ratings" :key="item.rateTime" class="rating-item border-1px">
            <div class="avatar">
              <img :src="item.avatar">
            </div>
            <div class="rating-detail">
              <span class="username">{{item.username}}</span>
              <span class="rateTime">{{item.rateTime}}rateTime</span>
              <show-star :size=36 :score="item.score"></show-star>
              <div class="text">{{item.text}}</div>
              <div class="recommend" v-if="item.recommend.length>0">
                <i class="icon-thumb_up"></i>
                <span class="recommend-goods" v-for="i in item.recommend" :key="i">{{i}}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import showStar from '../star/showStar'
import RatingSelect from '../ratingselect/ratingSelect'
export default {
  name: 'Rating',
  components: {
    showStar,
    RatingSelect
  },
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      ratings: {}
    }
  },
  methods: {
    // 获取ratings json 数据
    getInfo () {
      axios.get('/api/ratings')
        .then(this.getRatings)
    },
    getRatings (res) {
      // console.log(res)
      res = res.data
      if (res.errno === 0 && res.data) {
        const data = res.data
        this.ratings = data
        this.len = this.ratings.length
        // console.log(this.ratings)
        // $nextTick() 为正确计算高度
        this.$nextTick(() => {
          this._initScroll()
        })
      }
    },
    _initScroll () {
      this.ratingScroll = new BScroll(this.$refs.rating, {
        click: true,
        tap: true
      })
    }
  },
  created () {
    this.getInfo()
  }
}
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin.styl"
.Rating
  position absolute
  top 10.875rem /* 174/16 */
  bottom 0
  width 100%
  overflow hidden
  background #FCC
  .rating-head
    padding 18px 24px
    .left
      float left
      margin-right 24px
      text-align center
      .score
        margin 6px 0
        line-height 28px
        font-size 24px
        color rgb(255, 153, 0)
      .title
        margin 8px 0
        line-height 12px
        font-size 12px
        color rgb(7, 17, 27)
      .compare
        margin 6px 0
        line-height 10px
        font-size 10px
        color rgba(7, 17, 27, 0.6)
    .right
      float left
      .score-wrapper
        margin-top 8px
        div
          float left
          line-height 18px
          font-size 12px
        .title
          margin-right 12px
          color rgb(7, 17, 27)
        .score
          margin-left 12px
          color rgb(255, 153, 0)
        .time
          color rgb(147, 153, 159)
  .rating-select-wrapper
    box-sizing border-box
    width 100%
    padding 0 18px
  .rating-content
    .rating-item
      padding 18px
      background #fcf
      border-1px(rgba(7, 17, 27, 1))
      .avatar
        float left
        margin-right 12px
        width 28px
        height 28px
        border-radius 50%
        overflow hidden
        img
          width 100%
      .rating-detail
        position relative
        margin-left 40px
        .username
          line-height 12px
          font-size 10px
          color rgb(7, 17, 27)
        .rateTime
          text-align right
          line-height 12px
          font-size 10px
          font-weight 200
          color rgb(147, 153, 159)
        .text
          line-height 18px
          font-size 12px
          color rgb(7, 17, 27)
        .recommend
          font-size 0
          .icon-thumb_up:before
            line-height 16px
            font-size 12px
            color rgb(0, 160, 220)
          .recommend-goods
            display inline-block
            padding 0 6px
            line-height 16px
            font-size 9px
            color rgb(147, 153, 159)
            margin-left 8px
</style>












