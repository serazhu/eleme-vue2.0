<template>
  <div>
    <myheader :seller="seller"></myheader>
    <div class="menu">
      <div class="menu-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="menu-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="menu-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import axios from 'axios'
import myheader from 'components/header/myheader'

export default {
  data () {
    return {
      seller: {}
    }
  },
  components: {
    myheader
  },
  created () {
    axios.get('static/data.json').then((res) => {
      this.seller = res.data.seller
    }).catch(error => console.log(error))
  }
}
</script>

<style lang="stylus">
@import 'common/stylus/mixin.styl';
.menu
  display: flex
  .menu-item
    flex: 1
    text-align: center
    line-height: 40px
    border-1px(rgba(7,17,27,.1))
    & > a
      display: block
      color:rgb(77,85,93)
      font-size 14px
      &.active
        color: rgb(240,20,20)
    </style>
