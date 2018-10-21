<template>
  <transition>
    <slot></slot>
  </transition>
</template>

<script>
export default {
  name: 'SlideUpAnimation'
}
</script>

<style lang="stylus" scoped>
  .v-enter, .v-leave-to
    transform translate3d(0, 100%, 0)
  .v-enter-to, .v-leave
    transform translate3D(0, 0%, 0);
  .v-enter-active, .v-leave-active
    transition all .5s linear
</style>
