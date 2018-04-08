<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div class="supports" v-if="seller.supports">
          <span class="supports-icon" :class="iconClassMap[seller.supports[0].type]"></span>
          <span class="supports-text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="content-support" @click="showdetail" v-if="seller.supports">{{seller.supports.length}}个<i class="icon-keyboard_arrow_right"></i></div>
    </div>
    <div class="notice" @click="showdetail">
      <span class="notice-img"></span><span class="notice-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>     
    </div>
    <div class="background"><img :src="seller.avatar" width="100%" height="100%"></div>
    <transition name="fade">
      <div class="detail" v-show="show">
        <div class="detail-wrapper">
          <div class="detail-main clearfix">
            <div class="detail-name">{{seller.name}}</div>
            <div class="stars">
              <star :size="48" :score="seller.score"></star>        
            </div>
            <div class="title">
              <div class="title-line"></div>
              <div class="title-text">优惠信息</div>
              <div class="title-line"></div>
            </div>
            <ul class="descriptions">
              <li v-for="(item, index) in seller.supports" :key="index"><i :class="iconClassMap[item.type]"></i><span>{{item.description}}</span></li>
            </ul>
            <div class="title">
              <div class="title-line"></div>
              <div class="title-text">商家公告</div>
              <div class="title-line"></div>
            </div>
            <p class="bulletin">{{seller.bulletin}}</p>
          </div>
                  
        </div>
        <div class="close" @click="hidedetail"><i class="icon-close"></i></div>      
      </div>
    </transition>    
  </div>
</template>
<script>
import star from 'components/star/star'

export default{
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      show: false
    }
  },
  created () {
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components: {
    star
  },
  methods: {
    showdetail: function () {
      this.show = true
    },
    hidedetail: function () {
      this.show = false
    }
  }
}
</script>
<style lang="stylus">
@import '../../common/stylus/mixin.styl'

.header
  position: relative
  color:#fff
  background-color: rgba(7,17,27,.5)
  overflow: hidden
  .content-wrapper
    padding: 24px 12px 18px 24px    
    position: relative
    font-size: 0
    .avatar
      display: inline-block
      img
        border-radius: 2px
    .content
      display: inline-block
      padding-left: 16px
      vertical-align: top
      .title
        padding:2px 0 8px
        .brand
          bg-img(brand)
          background-size: 30px 18px
          background-repeat: no-repeat
          width: 30px
          height: 18px
          display: inline-block
          vertical-align: top
        .name
          padding-left: 6px
          font-size: 16px
          font-weight: bold
          line-height: 18px
      .description
        font-size: 12px
        font-weight: 20
    .supports
      padding-top:10px
      .supports-icon
        width: 12px
        height: 12px
        background-size: 12px 12px
        background-repeat: no-repeat
        display: inline-block
        vertical-align: top
        &.decrease
          bg-img(decrease_1)
        &.discount
          bg-img(discount_1)
        &.special
          bg-img(special_1)
        &.invoice
          bg-img(invoice_1)
        &.guarantee
          bg-img(guarantee_1)
      .supports-text
        font-size: 10px
        padding-left: 4px
        line-height:12px
    .content-support
      position: absolute
      right: 12px
      bottom: 14px
      height: 24px
      padding: 0 8px
      font-size: 10px
      line-height: 24px
      background-color: rgba(0,0,0,.2)
      border-radius: 14px
      .icon-keyboard_arrow_right
        line-height: 24px
        margin-left: 2px
  .notice
    height: 28px
    background-color: rgba(7,17,27,.2)
    padding: 0 22px 0 12px
    white-space: nowrap
    text-overflow: ellipsis
    overflow: hidden
    position: relative
    line-height: 28px
    .notice-img
      width: 22px
      height: 12px
      bg-img(bulletin)
      background-size: 22px 12px
      display: inline-block
      vertical-align: top
      margin-top: 8px
    .notice-text
      font-size: 10px
      margin: 0 4px
      vertical-align: top
    i 
      position:absolute
      right:12px
      top: 8px
      font-size: 10px       
  .background
    position: absolute
    left: 0    
    top: 0
    filter: blur(10px)
    z-index: -1
    width: 100%
    height: 100%
  .detail
    position: fixed
    top: 0
    left: 0
    z-index: 9
    background:rgba(7,17,27,.8)
    width: 100%
    height: 100%
    color: #fff
    overflow: auto
    transition: all .5s
    .detail-wrapper
      min-height:100%      
      .detail-main
        padding:64px 0 64px
        .detail-name
          font-size: 16px
          text-align: center 
          font-weight: 700
        .title
          padding:0 36px 24px
          display:flex
          .title-line
            flex:1
            background-color: rgba(255,255,255,.2)
            height:1px
            margin-top:6px
          .title-text
            padding:0 12px
            font-size: 14px
            font-weight: 700
        .descriptions
          padding-left: 36px
          padding-bottom: 14px
          li
            font-size: 0
            padding-bottom: 12px
            i
              width: 16px
              height: 16px
              margin-right: 6px
              display: inline-block
              background-size: 100% 100%
              vertical-align: top 
              &.decrease
                bg-img(decrease_1)
              &.discount
                bg-img(discount_1)
              &.special
                bg-img(special_1)
              &.invoice
                bg-img(invoice_1)
              &.guarantee
                bg-img(guarantee_1)
            span
              font-size: 12px
              line-height: 16px
        .bulletin
          font-size:12px    
          font-weight: 200
          line-height: 24px
          padding:0 48px 
    .close
      text-align: center
      font-size: 32px
      color: rgba(255,255,255,.5)
      position: relative 
      margin-top: -64px
    &.fade-enter-active, &.fade-leave-active
      transition:opacity .5s
    &.fade-enter, &.fade-leave-to
      opacity: 0
</style>
