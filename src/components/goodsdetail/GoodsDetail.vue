<template>
<slide-left-animation>
  <div class="detailWrapper" ref="detailWrapper"  v-show="isShow">
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
      </div>
    </div>
  </div>
</slide-left-animation>
</template>

<script>
import Vue from 'vue'
import SlideLeftAnimation from '../animate/SlideLeftAnimation'
import CartControl from '../cartcontrol/CartControl'
import BScroll from 'better-scroll'
export default {
  name: 'GoodsDetail',
  components: {
    SlideLeftAnimation,
    CartControl
  },
  props: {
    selectedGood: {
      type: Object
    }
  },
  data () {
    return {
      isShow: false
    }
  },
  methods: {
    show () {
      this.isShow = true
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
    hideGoodsDetail () {
      this.isShow = false
    },
    addFirstGoods (event) {
      if (!event._constructed) {
        return
      }
      Vue.set(this.selectedGood, 'count', 1)
      this.isFirst = false
    }
  },
  computed: {
    isControl () {
      if (this.selectedGood.count > 0) {
        return true
      } else {
        return false
      }
    }
  }

}
</script>

<style lang="stylus" scoped>
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
      background #fcc
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
    
</style>
