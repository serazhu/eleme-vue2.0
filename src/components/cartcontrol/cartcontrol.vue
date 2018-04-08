<template>  
  <div class="cartcontrol-wrapper">
    <transition name="show">
      <div class="cart-decrease" v-if="food.count > 0" @click="decreaseCart" ><i class="icon-remove_circle_outline inner"></i></div>
    </transition>
    <div class="cart-count" v-if="food.count > 0">{{food.count}}</div>
    <div class="cart-add" @click="addCart"><i class="icon-add_circle"></i></div>
  </div>
</template>
<script>
import Vue from 'vue'
export default {
  data () {
    return {
      
    }
  },
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    decreaseCart () {
      this.food.count--
    },
    addCart () {      
      if (!this.food.count) {
        Vue.set(this.food, 'count', 0)
      }
      this.food.count++  
      this.$emit('add')    
    }
  }
  
}
</script>
<style lang="stylus">
.cartcontrol-wrapper
  position: absolute
  right: 6px  
  bottom: 24px
  font-size: 0            
  .cart-add
    font-size: 24px
    color: rgb(0,160,220)
    display: inline-block 
  .cart-decrease
    display: inline-block
    opacity: 1
    transform: translate3d(0, 0, 0)  
    .inner
      display: inline-block
      color: rgb(0,160,220)
      font-size: 24px
      transition all .4s linear
      transform: rotate(0)
    &.show-enter,&.show-leave-to
      opacity: 0
      transform: translate3d(24px,0,0)
      .inner 
        transform: rotate(180deg)
        transition: all .4s linear
    &.show-enter-active,&.show-leave-active      
      transition: all .4s linear
  .cart-count  
    font-size: 10px
    color: rgb(147,153,159) 
    line-height: 24px
    display: inline-block  
    width: 24px
    text-align:center
    vertical-align: top  
</style>
