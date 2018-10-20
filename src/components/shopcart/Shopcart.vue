<template>
  <div class="Shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
          </div>
          <div class="num" v-if="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price border-1px-x" :class="{'highlight':totalCount>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="payClass">
        <div class="pay">{{payDesc}}</div>
      </div>
      <!-- 小球掉落动画 -->
      <div class="ball-container">
        <div class="ball"
              v-for="(ball, index) in balls" :key="index"
              v-show="ball.show"
              transition="drop">
          <div class="inner"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Shopcart',
  components: {},
  props: {
    deliveryPrice: {
      type: Number
    },
    minPrice: {
      type: Number
    },
    selectFoods: {
      type: Array,
      default () {
        return [
          {
            price: 0,
            count: 0
          }
        ]
      }
    }
  },
  data () {
    return {
      // 小球当前状态
      balls: [
        {
          show: false
        }, {
          show: false
        }, {
          show: false
        }, {
          show: false
        }, {
          show: false
        }
      ],
      dropBalls: []
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}起送`
      } else if (this.totalPrice > 0 && this.totalPrice < this.minPrice) {
        let extra = this.minPrice - this.totalPrice
        return `还差￥${extra}起送`
      } else if (this.totalPrice >= this.minPrice) {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice >= this.minPrice) {
        return 'enough'
      } else {
        return 'not-enough'
      }
    }
  },
  methods: {
    drop (el) {
      // console.log(el)
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = el
          this.dropBalls.push(ball)
          return
        }
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
  .Shopcart
    position fixed
    bottom 0
    width 100%
    height 3rem /* 48/16 */
    z-index 50
    .content
      display flex
      .content-left
        flex 1
        background #141d27
        font-size 0
        .logo-wrapper
          display inline-block
          position relative
          box-sizing border-box
          top -.625rem /* 10/16 */
          margin 0 12px
          padding 6px
          width 56px
          height 3.5rem /* 56/16 */
          vertical-align top
          border-radius 50%
          background #141d27
          .logo
            width 100%
            height 100%
            text-align center
            border-radius 50%
            background #2b343c
            &.highlight
              background rgb(0, 160, 220)
            .icon-shopping_cart
              line-height 3rem /* 48/16 */
              font-size 1.5rem /* 24/16 */
              color #80858a
              &.highlight
                color #fff
          .num
            position absolute
            top 0
            right 0
            width 1.5rem /* 24/16 */
            height 1rem /* 16/16 */
            line-height 1rem /* 16/16 */
            text-align center
            font-size .5625rem /* 9/16 */
            font-weight 700
            color #fff
            background rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
            border-radius 1rem /* 16/16 */
        .price
          display inline-block
          vertical-align top
          margin-top .75rem /* 12/16 */
          line-height 1.5rem /* 24/16 */
          font-size 1rem /* 16/16 */
          font-weight 700
          color rgba(255, 255, 255, 0.4)
          &.highlight
            color #fff
          &:after
            display inline-block
            vertical-align top
            margin 0 .5rem /* 8/16 */
            content " "
            height 1.5rem /* 24/16 */
            border-left 1px solid rgba(255, 255, 255, 0.4)
        .desc
          display inline-block
          vertical-align top
          margin-top .75rem /* 12/16 */
          line-height 1.5rem /* 24/16 */
          font-size .625rem /* 10/16 */
          color rgba(255, 255, 255, 0.4)
      .content-right
        flex 0 0 105px
        box-sizing border-box
        width 105px
        padding 0 8px
        background #2b333b
        .pay
          height 3rem /* 48/16 */
          line-height 3rem /* 48/16 */
          text-align center
          font-size .75rem /* 12/16 */
          font-weight 700
        &.not-enough
          background #2b333b
          color rgba(255, 255, 255, 0.4)
        &.enough
          background #00b43c
          color #fff
    .ball-container
      .ball
        position fixed
        left 2rem /* 32/16 */
        bottom 2rem /* 32/16 */
        z-index 200
        &.drop-transition
          transition all .4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
          .inner
            width 16px
            height 16px
            border-radius 50%
            background rgb(0, 160, 220)
            transition all .4s linear
</style>
