<template>
  <div class="header">
    <!-- 内容 -->
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime+'分钟送达'}}
        </div>
        <!-- 优惠信息 -->
        <!-- 数据获取是异步过程，最开始是sell空对象传给header,seller.supports是undefined会报错，故用v-if判断 -->
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <!-- 公告 -->
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span
      ><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <!-- 背景图片 -->
    <div class="background">
      <img :src="seller.avatar" alt="">
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
export default {
  name: 'Header',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
    
    }
  },
  created () {
    // support部分左侧小图标对应的class
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'gurantee']
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.header
  position relative
  background rgba(7, 17, 27, 0.5)
  color #fff
  .content-wrapper
    position relative
    padding 24px 12px 18px 24px
    font-size 0
    .avatar
      display inline-block
      vertical-align top
      img
        width 4rem /* 64/16 */
        height 4rem /* 64/16 */
        border-radius 2px
    .content
      display inline-block
      margin-left 16px
      font-size 14px
      .title
        margin 2px 0 8px 0
        .brand
          display inline-block
          width 30px
          height 18px
          vertical-align top
          bg-image('brand')
          background-size 100% 100%
          background-repeat no-repeat
        .name
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
      .description
        margin-bottom 10px
        line-height 12px
        font-size 12px
      .support
        .icon
          display inline-block
          width 12px
          height 12px
          margin-right 4px
          vertical-align top
          background-size 100% 100%
          background-repeat no-repeat
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.gurantee
            bg-image('guarantee_1')
          &.invoice
            bg-image('invoice_1')
          &.special
            bg-image('special_1')
        .text
          vertical-align top
          font-size 10px
          line-height 12px
    .support-count
      position absolute
      right 12px
      bottom 18px
      display inline-block
      padding 0 8px
      height 24px
      line-height 24px
      border-radius 14px
      background-color rgba(0, 0, 0, 0.2)
      // background-color rgba(255, 0, 0, 1)
      .count,.icon-keyboard_arrow_right
        vertical-align middle
        font-size 10px
        line-height 12px
      .count
        padding-left 2px
  .bulletin-wrapper
    position relative
    box-sizing border-box
    overflow hidden
    height 1.75rem /* 28/16 */
    line-height 1.75rem /* 28/16 */
    padding 0 22px 0 12px
    white-space nowrap
    text-overflow ellipsis
    background-color rgba(7, 17, 27, 0.2)
    .bulletin-title
      display inline-block
      vertical-align middle
      width 22px
      height 12px
      bg-image('bulletin')
      background-size 100% 100%
    .bulletin-text
      vertical-align middle
      font-size 10px
      margin 0 4px
    .icon-keyboard_arrow_right
      position absolute
      right .75rem /* 12/16 */
      font-size 10px
      height 1.75rem /* 28/16 */
      line-height 1.75rem /* 28/16 */
  .background
    position absolute
    top 0
    left 0
    width 100%
    height 100%
    filter blur(10px)
    z-index -1
    img
      width 100%
      height 100%
</style>
