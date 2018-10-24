<template>
  <transition>
    <slot></slot>
  </transition>
</template>

<script>
export default {
  name: 'SlideLeftAnimation'
}
</script>

<style lang="stylus" scoped>
  .v-enter, .v-leave-to
    transform translate3d(100%, 0, 0)
  .v-enter-to, .v-leave
    transform translate3D(0%, 0, 0);
  .v-enter-active, .v-leave-active
    transition all 0.5s linear
</style>
