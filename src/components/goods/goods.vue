<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuwrapper">
      <ul>
        <li v-for="(good, index) in goods" :key="index" :class="{current:currentIndex===index}" @click="selectMenu(index)">
          <span class="text">
            <span class="icon" v-if="good.type > 0" :class="iconClassMap[good.type]"></span>
            {{good.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodswrapper">
      <ul>
        <li class="food-list" v-for="(good, index) in goods" :key="index"  ref="foodList">
          <h1>{{good.name}}</h1>
          <ul>
            <li class="food-item" v-for="(food, index) in good.foods" :key="index">
              <div class="icon">
                <img :src="food.icon" alt=""  width="57" height="57">
              </div>
              <div class="content">
                <h2>{{food.name}}</h2>
                <p v-show="food.description">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span class="rate">好评率{{food.rating}}%</span>  
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
              <cartcontrol @add="add" :food="food"></cartcontrol>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="shorpcart">
      <div class="content-left">
        <div class="cart"><i class="icon-shopping_cart"></i><i class="num">10</i></div>
        <div class="price">￥10</div>
        <div class="delivery-price">另需配送费4元</div>
      </div>
      <div class="content-right">
        <a href="#" class="min-price">20元起送</a>
      </div>
    </div>
  </div>
  
</template>
<script>
import axios from 'axios'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import BScroll from 'better-scroll'

export default {
  data () {
    return {
      goods: [],
      count: 0,
      listHeight: [],
      scrollY: 0
    }
  },
  created () {
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']

    axios.get('static/data.json').then((res) => {
      this.goods = res.data.goods
      this.$nextTick(() => {
        this._initScroll()
        // 获取每个类高度的数组
        this._calculateHeight()
      })
    }).catch(function () {
      console.log('there is something wrong')
    })   
  },
  components: {
    cartcontrol: cartcontrol
  },
  computed: {
    currentIndex () {      
      for (var i = 0, len = this.listHeight.length; i < len; i++) {
        var height1 = this.listHeight[i]
        var height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    }
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuwrapper, {})
      this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
        probeType: 3
      })
      // 监听滚动事件，获取滚动高度
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    add () {
      
    },
    selectMenu(index, event){

    },
    _calculateHeight () {
      let foodList = this.$refs.foodList      
      var itemHeight = 0
      this.listHeight.push(itemHeight)      
      for (var i = 0, len = foodList.length; i < len; i++) {
        let item = foodList[i]
        itemHeight += item.clientHeight
        this.listHeight.push(itemHeight)
      }
    }
  }
} 
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl'
.goods
  position: absolute 
  top: 174px
  bottom: 48px
  display: flex
  width: 100%
  overflow: hidden
  .menu-wrapper
    width:80px
    flex: 0 0 80px
    ul
      li
        background-color: #f3f5f7
        padding: 0 12px      
        line-height: 14px
        width: 56px
        height: 54px
        display: table
        .text
          font-size: 12px
          color: #07111b
          display: table-cell
          height: 54px
          border-1px(rgba(7,17,27,.1))
          vertical-align: middle
          .icon
            width: 12px
            height: 12px
            display: inline-block
            background-size: 100% 100%
            background-repeat: no-repeat
            vertical-align: top 
          .decrease          
            bg-img(decrease_3)
          .discount          
            bg-img(discount_3)
          .special          
            bg-img(special_3)
          .invoice          
            bg-img(invoice_3)
          .guarantee          
            bg-img(guarantee_3)
          .name
            font-size: 12px
        &.current
          background: #ffffff
          font-weight: 700 
          margin-top:-1px
          position: relative 
          z-index: 9         
          .text
            border-none()
  .foods-wrapper
    flex: 1
    .food-list
      h1
        font-size: 12px
        color: rgb(147,153,159)
        background-color: #f3f5f7
        border-left: 2px solid #d9dde1
        line-height: 26px
        padding-left: 14px
      ul        
        .food-item
          display: flex
          margin: 18px 
          position: relative
          border-1px(rgba(7,17,27,.1))
          padding-bottom: 18px
          &:last-child
            border-none()
            margin-bottom: 0
          .icon
            flex:0 0 57px
            margin-right: 10px
          .content
            flex: 1
            h2
              font-size: 14px
              padding-top: 2px
            p,.extra
              font-size: 10px
              color: rgb(147,153,159)
              padding-top: 8px
            .extra 
              padding-top: 8px
              font-size: 0
              .count
                padding-right: 12px  
                font-size: 10px
              .rate
                font-size: 10px              
            .price
              font-size: 0
              line-height: 24px
              font-weight: 700
              .now
                color:rgb(240,20,20)
                font-size: 14px                
                padding-right: 8px
                line-height: 24px
              .old  
                color: rgb(147,153,159)
                font-size: 10px
                text-decoration:line-through                       
  .shorpcart
    position: fixed 
    width: 100%
    height: 48px
    left: 0
    bottom: 0
    background: #141d27
    display:flex
    .content-left
      flex:1
      font-size: 0
      .cart
        border-radius: 100%
        background: #2b343c
        width: 44px
        height: 44px
        position: absolute
        border:6px solid #141d27 
        top: -10px
        left: 12px
        display: inline-block 
        font-size: 0
        .icon-shopping_cart
          font-size: 24px
          color: rgba(255,255,255,.4)
          text-align: center 
          display: inline-block
          width: 100%
          height: 100%
          line-height: 44px 
        .num 
          font-size: 9px
          color: #ffffff
          width: 24px
          line-height: 16px
          position: absolute
          right: -6px
          background: rgb(240,20,20)
          border-radius: 16px
          text-align: center
          font-style: normal
          box-shadow: 0 4px 8px rgba(0,0,0,.4)
          top: -6px
      .price 
        display: inline-block
        color: rgba(255,255,255,.4)
        font-weight: 700
        font-size: 16px
        margin:12px 0 12px 80px
        line-height: 24px
        border-right: 1px solid rgba(255,255,255,.1)
        padding-right:12px
      .delivery-price
        display: inline-block
        color: rgba(255,255,255,.4)
        font-size: 10px
        line-height: 48px
        padding-left: 12px
    .content-right
      flex: 0 0 105px
      width: 105px
      .min-price
        font-size: 12px
        font-weight: 700
        color: rgba(255,255,255,.4)
        display: inline-block
        width: 105px
        background: #2b333b
        height: 48px
        text-align: center
        line-height: 48px
</style>
