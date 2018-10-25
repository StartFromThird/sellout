<template>
  <div class="RatingSelect">
    <div class="rating-type clearfloat">
      <div class="type all" :class="{'active':selectType===2}" @click="select(2, $event)">
        {{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </div>
      <div class="type positive" :class="{'active':selectType===0}" @click="select(0, $event)">
        {{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </div>
      <div class="type negative" :class="{'active':selectType===1}" @click="select(1, $event)">
        {{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </div>
    </div>
    <div class="switch border-1px" @click="toggleOnlyContent($event)">
      <span class="icon-check_circle" :class="{'active':onlyContent}"></span>
      <span class="text">只看有内容评价</span>
    </div>
  </div>
</template>

<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
export default {
  name: 'RatingSelect',
  components: {},
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  data () {
    return {
    }
  },
  computed: {
    // 计算positive评价
    positives () {
      return this.ratings.filter((i) => {
        return i.rateType === POSITIVE
      })
    },
    // 计算negative评价
    negatives () {
      return this.ratings.filter((i) => {
        return i.rateType === NEGATIVE
      })
    }
  },
  methods: {
    // 选择查看 i 类评价， POSITIVE = 0, NEGATIVE = 1, ALL = 2
    select (i, event) {
      if (!event._constructed) {
        return
      }
      this.$emit('changeType', i)
    },
    // 是否只看有内容评价
    toggleOnlyContent (event) {
      if (!event._constructed) {
        return
      }
      this.$emit('changeOnly', !this.onlyContent)
    }
  }
}
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin.styl"
.RatingSelect
  .rating-type
    margin 18px 0
    .type
      float left
      margin-right 8px
      padding 8px 12px
      height 16px
      line-height 16px
      text-align center
      font-size 12px
      border-radius 1px
      .count
        margin-left 2px
        font-size 8px
      &.all
        color rgb(77, 85, 93)
        background rgba(0, 160, 220, 0.2)
        &.active
          color #fff
          background rgb(0, 160, 220)
      &.positive
        color rgb(77, 85, 93)
        background rgba(0, 160, 220, 0.2)
        &.active
          color #fff
          background rgb(0, 160, 220)
      &.negative
        color rgb(77, 85, 93)
        background rgba(77, 85, 93, 0.2)
        &.active
          color #fff
          background rgb(77, 85, 93)
  .switch
    // outline 1px solid red
    margin 0 -18px
    padding 12px 18px
    border-1px(rgba(7, 17, 27, 0.1))
    border-1px(rgba(7, 17, 27, 1))
    .icon-check_circle,.text
      line-height 24px
      color rgb(147, 153, 159)
      vertical-align middle
    .icon-check_circle
      font-size 24px
      margin-right 4px
      display inline-block
      &.active
          color #00c850
</style>
