<template>
  <div class="goods">
    <!-- 左侧分类导航 -->
    <div class="menu-wrapper">
      <ul>
        <li class="menu-item" v-for="(item) in goods" :key="item.name">
          <span class="text border-1px">
            <span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <!-- 右侧商品信息 -->
    <div class="foods-wrapper">
      <ul>
        <li class="food-list" v-for="(item) in goods" :key="item.name">
          <p class="title">{{item.name}}</p>
          <ul>
            <li class="food-item border-1px" v-for="i in item.foods"  :key="i.name">
              <div class="icon">
                <img :src="i.icon" alt="">
              </div>
              <div class="content">
                <p class="name">{{i.name}}</p>
                <p class="desc">{{i.description}}</p>
                <div class="extra">
                  <span class="count">月售{{i.sellCount}}份</span>
                  <span>好评率{{i.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{i.price}}</span>
                  <span class="old" v-if="i.oldPrice">￥{{i.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
// 接口数据返回正确
const ERR_OK = 0
export default {
  name: 'Goods',
  components: {},
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: []
    }
  },
  methods: {
    // 获取goods json 数据
    getInfo () {
      axios.get('/api/goods')
        .then(this.getGoods)
    },
    getGoods (res) {
      // console.log(res)
      res = res.data
      if (res.errno === ERR_OK && res.data) {
        const data = res.data
        this.goods = data
        console.log(this.goods)
      }
    },
    // 初始化better-scroll
    _initScroll () {
      this.meunScroll = new BScroll()
    }

  },
  created () {
    this.getInfo()
    // support部分左侧小图标对应的class
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  }
}
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin.styl"
.goods
  display flex
  position absolute
  top 10.875rem /* 174/16 */
  bottom 2.875rem /* 46/16 */
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 5rem /* 80/16 */
    width 5rem /* 80/16 */
    background #f3f5f7
    // background #fcf
    .menu-item
      display table
      height 54px
      width 3.5rem /* 56/16 */
      padding 0 .75rem /* 12/16 */
      line-height 14px
      .icon
        display inline-block
        width 16px
        height 16px
        vertical-align top
        margin-right 2px
        background-size 100% 100%
        background-repeat no-repeat
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
      .text
        // 多行文本垂直对齐
        display table-cell
        vertical-align middle
        width 56px
        border-1px(rgba(7, 17, 27, 0.1))
        // border-1px(rgba(7, 17, 27, 1))
        font-size 12px
  .foods-wrapper
    flex 1
    background #fcf
    .title
      padding-left 14px
      height 26px
      line-height 26px
      border-left 2px solid #d9dde1
      // border-left 2px solid #f00
      font-size 12px
      color rgb(147, 153, 159)
      background #f3f5f7
    .food-item
      display flex
      margin 18px
      padding-bottom 18px
      border-1px(rgba(7, 17, 27, 0.1))
      border-1px(rgba(7, 17, 27, 1))
      &:last-child
        border-none()
        margin-bottom 0
      .icon
        flex 0 0 57px
        margin-right 10px
        img
          width 57px
          height 57px
      .content
        flex 1
        .name
          margin 2px 0 8px 0
          height 14px
          line-height 14px
          font-size 14px
          color rgb(7, 17, 27)
        .desc, .extra
          line-height 10px
          font-size 10px
          color rgb(147, 153, 159)
        .desc
          line-height 12px
          margin-bottom 8px
        .extra
          .count
            margin-right 12px
        .price
          font-weight 700
          line-height 24px
          .now
            margin-right 8px
            font-size 14px
            color rgb(240, 20, 20)
          .old
            text-decoration line-through
            font-size 10px
            color rgb(147, 153, 159)

</style>
