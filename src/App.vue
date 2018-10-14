<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
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
import axios from 'axios'
// 接口数据返回正确
const ERR_OK = 0
export default {
  name: 'App',
  components: {
    'v-header': Header
  },
  data () {
    return {
      seller: {}
    }
  },
  methods: {
    getInfo () {
      axios.get('/api/seller')
        .then(this.getSeller)
    },
    getSeller (res) {
      // console.log(res)
      res = res.data
      if (res.errno === ERR_OK && res.data) {
        const data = res.data
        this.seller = data
        // console.log(this.seller)
      }
    }
  },
  created () {
    this.getInfo()
  }
}
</script>

<style lang="stylus" scoped>
// 这里不能引入icon.styl相关, icon.styl要在 main.js import
@import "./common/stylus/mixin.styl"
#app
  .tab
    display flex
    width 100%
    height  2.5rem /* 40/16 */
    line-height 2.5rem /* 40/16 */
    // border-bottom 1px solid rgba(7, 17, 27, 0.1)
    // border-1px(rgba(7, 17, 27, 0.1))
    border-1px(rgba(7, 17, 27, 1))
    .tab-item
      flex 1
      text-align center
      &>a
        display block
        font-size 14px
        color rgb(77, 85, 93)
        text-decoration none
        // outline 1px solid red
        &.active
          color rgb(240, 20, 20)

</style>
