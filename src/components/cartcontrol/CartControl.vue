<template>
  <div class="CartControl">
    <roll-out-animation>
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart">
    </div>
    </roll-out-animation>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue'
import RollOutAnimation from '../animate/RollOutAnimation'
export default {
  name: 'CartControl',
  components: {
    RollOutAnimation
  },
  props: {
    food: {
      type: Object
    }
  },
  data () {
    return {

    }
  },
  methods: {
    // 这里直接改了父元素的food对象
    decreaseCart (event) {
      if (!event._constructed) {
        return
      }
      if (this.food.count <= 1) {
        this.food.count = 0
      } else {
        this.food.count--
      }
    },
    addCart (event) {
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        // 给food增加count属性，给值1
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      // 对应target传给父组件Goods
      this.$emit('addFood', event.target)
    }
  }
}
</script>

<style lang="stylus" scoped>
.CartControl
  font-size 0
  .cart-decrease
    display inline-block
    padding 6px
    line-height 24px
    font-size 24px
    color rgb(0, 160, 220)
  .cart-add
    display inline-block
    padding 6px
    line-height 24px
    font-size 24px
    color rgb(0, 160, 220)
  .cart-count
    display inline-block
    width 12px
    padding-top 6px
    vertical-align top
    text-align center
    line-height 24px
    font-size 10px
    color rgb(147, 153, 159)
</style>
