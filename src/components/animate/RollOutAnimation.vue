<template>
  <transition>
    <slot></slot>
  </transition>
</template>

<script>
export default{
  name: 'RollOutAnimation'
}
</script>

<style lang="stylus" scoped>
  .v-enter, .v-leave-to
    opacity 0
    transform translate3d(24px, 0, 0)
    transform rotate(180deg)
    // .inner
    //   transform rotate(180deg)
  .v-enter-active, .v-leave-active
    transition all 0.5s linear
    // 给子元素写动画貌似不能起效
    // .inner
    //   transition all 0.5s linear
</style>
