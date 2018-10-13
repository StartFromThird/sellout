<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>

<script>
import Header from './components/header/Header'
export default {
  name: 'App',
  components: {
    'v-header': Header
  }
}
</script>

<style lang="stylus" scoped>
#app
  .tab
    display flex
    width 100%
    height  2.5rem /* 40/16 */
    line-height 2.5rem /* 40/16 */
    .tab-item
      flex 1
      text-align center
      &>a
        display block
        text-decoration none
        outline 1px solid red
</style>
