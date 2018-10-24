<template>
  <div class="goods">
    <!-- 左侧分类导航 -->
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li class="menu-item" v-for="(item, index) in goods" :key="item.name" :ref="index"
        :class="{'current': menuIndex===index}"
        @click="selectMenu(item.name, index, $event)">
          <span class="text border-1px">
            <span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <!-- 右侧商品信息 -->
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list" v-for="(item) in goods" :key="item.name" :ref="item.name">
          <p class="title">{{item.name}}</p>
          <ul>
            <li class="food-item border-1px" v-for="i in item.foods" :key="i.name" @click="showGoodsDetail(i, $event)">
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
                <!-- 商品加减组件 -->
                <div class="cartcontrol-wrapper">
                  <cart-control :food="i" @addFood="_drop"></cart-control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!-- 购物车 -->
    <shopcart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"
              :selectFoods="selectFoods"
              ref="shopcart">
    </shopcart>
    <!-- 商品详情页 -->
    <goods-detail ref="goodsdetail" :selectedGood="selectedGoods"></goods-detail>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import Shopcart from '../shopcart/Shopcart'
import CartControl from '../cartcontrol/CartControl'
import GoodsDetail from '../goodsdetail/GoodsDetail'
// 接口数据返回正确
const ERR_OK = 0
export default {
  name: 'Goods',
  components: {
    Shopcart,
    CartControl,
    GoodsDetail
  },
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      // 右侧Y滑动距离
      scrollY: 0,
      len: 0,
      menuIndex: 0,
      selectedGoods: {}
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
        this.len = this.goods.length
        // console.log(this.goods)
        // $nextTick() 为正确计算高度
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    },
    // 初始化better-scroll
    _initScroll () {
      this.meunScroll = new BScroll(this.$refs.menuWrapper, {
        click: true,
        tap: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        probeType: 3,
        click: true,
        tap: true
      })
      // 获取foodsScroll的Y轴位移
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
        this.menuIndex = this.currentIndex
      })
    },
    // 获取右侧每个分类对应的clientHeight数组
    _calculateHeight () {
      let height = 0
      let len = this.len
      let foodList = []
      for (let i = 0; i < len; i++) {
        let name = this.goods[i].name
        foodList[i] = this.$refs[name][0]
        height = height + foodList[i].clientHeight
        this.listHeight.push(height)
      }
      this.listHeight.unshift(0)
      // console.log('list', foodList, this.listHeight)
    },
    // 点击左侧菜单，右侧跳到对应位置
    selectMenu (name, index, event) {
      // 清除自带click事件
      if (!event._constructed) {
        return
      }
      let ele = this.$refs[name][0]
      this.menuIndex = index
      this.foodsScroll.scrollToElement(ele)
      // this.foodsScroll.scrollToElement(ele, 300)
    },
    // 点击右侧菜单，显示对应商品详情
    showGoodsDetail (goods, event) {
      if (!event._constructed) {
        return
      }
      this.selectedGoods = goods
      this.$refs.goodsdetail.show()
    },
    _drop (target) {
      // 调用子组件drop方法 异步执行
      this.$nextTick(() => {
        this.$refs.shopcart.drop(target)
      })
    }
  },
  computed: {
    currentIndex () {
      let len = this.len
      for (let i = 0; i < len; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || ((this.scrollY >= height1) && (this.scrollY < height2))) {
          // console.log("Y", this.scrollY, height1, height2, i)
          return i
        }
      }
      return 0
    },
    // 遍历所有商品的food.count
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      // console.log(foods)
      return foods
    }
  },
  created () {
    this.getInfo()
    // support部分左侧小图标对应的class
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  mounted () {
    // 滚动
    // let menuWrapper = document.querySelector('.menu-wrapper')
    // // let menuWrapper = this.$refs.menuWrapper
    // this.meunScroll = new BScroll(menuWrapper, {
    //   click: true,
    //   tap: true
    // })
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
      &.current
        position relative
        z-index 10
        margin-top -1px
        background #fff
        font-weight 700
        .text
          border-none()
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
    // background #fcf
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
      // border-1px(rgba(7, 17, 27, 1))
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
        .cartcontrol-wrapper
          position absolute
          right 0
          bottom 12px
</style>
