<template>
  <div class="Star" :class="starType">
    <span class="star-item" :class="itemClass"
      v-for="(itemClass, index) in itemClasses" :key="index"></span>
    <!-- <span>{{score}}{{size}}</span> -->
  </div>
</template>

<script>
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'
export default {
  name: 'showStar',
  components: {},
  props: {
    // 传入单个星星大小 及 打分分数
    size: {
      type: Number,
      default: 36
    },
    score: {
      type: Number,
      required: true,
      validator: function (value) {
        return (value <= LENGTH) && (value > 0)
      }
    }
  },
  data () {
    return {
    }
  },
  computed: {
    starType () {
      return 'star-' + this.size
    },
    itemClasses () {
      let starArr = []
      let score = Math.floor(this.score * 2) / 2
      // 判断有无半星
      let isHalfStar = ((score % 1) !== 0)
      // 全星数量
      let onStarNum = Math.floor(score)
      // 填充星星数组
      for (let i = 0; i < onStarNum; i++) {
        starArr.push(CLS_ON)
      }
      if (isHalfStar) {
        starArr.push(CLS_HALF)
      }
      // 无星数量及填充到数组
      let offStarNum = LENGTH - starArr.length
      for (let i = 0; i < offStarNum; i++) {
        starArr.push(CLS_OFF)
      }
      // 打分爆表删除多余星星
      starArr.splice(LENGTH)
      // console.log(starArr)
      return starArr
    }
  }
}
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin.styl"
.Star
  .star-item
    display inline-block
    background-repeat no-repeat
  &.star-48
    .star-item
      width 20px
      height 20px
      margin-right 22px
      background-size 20px 20px
      &:last-child
        margin-right 0
      &.on
        bg-image('star48_on')
      &.half
        bg-image('star48_half')
      &.off
        bg-image('star48_off')
  &.star-36
    .star-item
      width 15px
      height 15px
      margin-right 6px
      background-size 100% 100%
      &:last-child
        margin-right 0
      &.on
        bg-image('star36_on')
      &.half
        bg-image('star36_half')
      &.off
        bg-image('star36_off')
  &.star-24
    .star-item
      width 10px
      height 10px
      margin-right 3px
      background-size 100% 100%
      &:last-child
        margin-right 0
      &.on
        bg-image('star24_on')
      &.half
        bg-image('star24_half')
      &.off
        bg-image('star24_off')
</style>
