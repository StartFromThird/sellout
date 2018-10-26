<template>
<slide-left-animation>
  <div class="detailWrapper" ref="detailWrapper"  v-if="isShow">
    <div class="GoodsDetail">
      <div class="imgheader">
        <img :src="selectedGood.image" >
        <div class="back" @click="hideGoodsDetail">
          <div class="icon-arrow_lift"></div>
        </div>
      </div>
      <div class="content clearfloat">
        <h1 class="goodsname">{{selectedGood.name}}</h1>
        <p class="sell">
          <span>月售{{selectedGood.sellCount}}份</span>
          <span>好评率{{selectedGood.rating}}%</span>
        </p>
        <div class="price">
          <span class="now">￥{{selectedGood.price}}</span>
          <span  class="old" v-if="selectedGood.oldPrice">￥{{selectedGood.oldPrice}}</span>
        </div>
        <div class="rightAdd">
          <div class="addToCart" v-show="!isControl" @click.stop.prevent="addFirstGoods">
            加入购物车
          </div>
          <div class="cartcontrol-wrapper" v-show="isControl">
            <cart-control :food="selectedGood"></cart-control>
          </div>
        </div>
      </div>
      <div class="splitBox" v-if="selectedGood.info"></div>
      <div class="content clearfloat" v-if="selectedGood.info">
        <h1 class="title">商品介绍</h1>
        <p class="info">{{selectedGood.info}}</p>
      </div>
      <div class="splitBox"></div>
      <div class="content clearfloat">
        <h1 class="title">商品评价</h1>
        <rating-select :ratings="selectedGood.ratings" :desc="desc"
        :selectType="selectType" @changeType="changeSelectType"
        :onlyContent="onlyContent" @changeOnly="changeOnlyContent"></rating-select>
        <div class="rating-content">
          <ul ref="selectedRatings">
            <li class="rating-item border-1px" v-for="rating in selectedGood.ratings" :key="rating.rateTime"
              v-show="needShow(rating.rateType, rating.text)">
              <div class="user">
                <span class="name">{{rating.username}}</span>
                <img :src="rating.avatar">
              </div>
              <div class="time">{{showDate(rating.rateTime)}}</div>
              <div class="detail">
                <span :class="{'icon-thumb_up':rating.rateType===0, 'icon-thumb_down':rating.rateType===1}"></span>
                <span class="text">{{rating.text}}</span>
              </div>
            </li>
          </ul>
          <div class="no-rating" v-show="isEmpty">暂无评论</div>
        </div>
      </div>

    </div>
  </div>
</slide-left-animation>
</template>

<script>
import Vue from 'vue'
import SlideLeftAnimation from '../animate/SlideLeftAnimation'
import CartControl from '../cartcontrol/CartControl'
import RatingSelect from '../ratingselect/ratingSelect'
import BScroll from 'better-scroll'
import {formatDate} from '../../common/js/date'
const ALL = 2
export default {
  name: 'GoodsDetail',
  components: {
    SlideLeftAnimation,
    CartControl,
    RatingSelect
  },
  props: {
    selectedGood: {
      type: Object
    }
  },
  data () {
    return {
      isShow: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      },
      selectType: ALL,
      onlyContent: false,
      isEmpty: false
    }
  },
  methods: {
    // 商品详情显示及selectType，onlyContent，scroll初始化
    show () {
      this.isShow = true
      this.selectType = ALL
      this.onlyContent = false
      this.$nextTick(() => {
        if (!this.scroll) {
          this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
            click: true,
            tap: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    // 点击左上角隐藏商品详情
    hideGoodsDetail () {
      this.isShow = false
    },
    // 第一次添加商品 给selectedGood新加count属性且设为1
    addFirstGoods (event) {
      if (!event._constructed) {
        return
      }
      Vue.set(this.selectedGood, 'count', 1)
    },
    // 更改查看 i 类评价
    changeSelectType (i) {
      this.selectType = i
      this.$nextTick(() => {
        this.detailWrapper.refresh()
        this.ratingHeight()
      })
    },
    // 更改是否 只看有内容评价
    changeOnlyContent (i) {
      this.onlyContent = i
      this.$nextTick(() => {
        this.detailWrapper.refresh()
        this.ratingHeight()
      })
    },
    // 判断是否显示该条评论
    needShow (type, text) {
      // 评论是否有内容
      let isContent = text.length > 0
      if (this.onlyContent && !isContent) {
        return false
      } else {
        if (this.selectType === ALL) {
          return true
        } else if (this.selectType === type) {
          return true
        } else {
          return false
        }
      }
    },
    // 计算显示评价时间
    showDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    },
    // 依据评论ul的高度判断评论是否为空
    ratingHeight () {
      let dom = this.$refs.selectedRatings
      if (dom) {
        let h = dom.clientHeight
        if (h < 10) {
          this.isEmpty = true
        } else {
          this.isEmpty = false
        }
      }
    }
  },
  computed: {
    // 是否显示cartcontrol还是显示加入购物车
    isControl () {
      if (this.selectedGood.count > 0) {
        return true
      } else {
        return false
      }
    }
  },
  mounted () {
    this.ratingHeight()
  }
}
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin.styl"
.detailWrapper
  position fixed
  top 0
  bottom 3rem /* 48/16 */
  width 100%
  background #fff
  z-index 30
  overflow hidden
  .imgheader
    position relative
    width 100%
    height 0
    padding-bottom 100%
    background #fcf
    img
      position absolute
      top 0
      // left 0
      width 100%
      height 100%
    .back
      position absolute
      top 10px
      left 10px
      background rgba(255, 255, 255, 0.2)
      width 40px
      height 40px
      border-radius 50%
      .icon-arrow_lift
        height 40px
        line-height 40px
        text-align center
        font-size 20px
        color #fff
  .content
    padding 18px
    // background #fcc
    .goodsname
      line-height 14px
      font-size 14px
      font-weight 700
      color rgb(7, 17, 27)
      margin-bottom 8px
    .sell
      margin-bottom 18px
      line-height 10px
      font-size 10px
      color rgb(147, 153, 159)
      span
        margin-right 12px
    .price
      float left
      line-height 24px
      font-weight 700
      .now
        margin-right 8px
        font-size 14px
        color rgb(240, 20, 20)
      .old
        font-size 10px
        color rgb(147, 153, 159)
        text-decoration line-through
    .rightAdd
      float right
      .addToCart
        box-sizing border-box
        height 24px
        padding 0 12px
        line-height 24px
        font-size 10px
        color #fff
        border-radius 12px
        background rgb(0, 160, 220)
    .title
      margin-bottom 6px
      line-height 14px
      font-size 14px
      color rgb(7, 17, 27)
    .info
      line-height 24px
      font-size 12px
      color rgb(77, 85, 93)
      font-weight 200
    .rating-content
      ul
        background #fff
        .rating-item
          padding 16px 0
          border-1px(rgba(7, 17, 27, 1))
          &:after
            display block
            height 0
            clear both
          .time
            line-height 12px
            font-size 10px
            color rgb(147, 153, 159)
          .user
            float right
            text-align right
            .name
              line-height 12px
              font-size 10px
              color rgb(147, 153, 159)
            img
              width 12px
              height 12px
              border-radius 50%
          .detail
            .icon-thumb_up,
            .icon-thumb_down
              line-height 24px
              font-size 12px
              color rgb(147, 153, 159)
            .icon-thumb_up
              color rgb(0, 160, 220)
            .text
              line-height 16px
              font-size 12px
              color rgb(7, 17, 27)
      .no-rating
        padding 16px 0 32px 0
        font-size 12px
        color rgb(147, 153, 159)
</style>
