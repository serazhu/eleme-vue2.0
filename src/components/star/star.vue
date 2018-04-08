<template>
<div class="star" :class="starSize">
    <span v-for="(item, index) in stars" :key="index" :class="item"  class="star-item"></span>
</div>
</template>
<script>
const LENGTH = 5

export default {
  data () {
    return {

    }    
  },
  props: {
    score: {
      type: Number
    },
    size: {
      type: Number
    }
  },
  computed: {
    stars: function () {
      var arr = []
      var newscore = Math.floor(this.score * 2) / 2
            
      for (var i = 0; i < Math.floor(newscore); i++) {
        arr.push('on')
      }          

      if (('' + this.score).match('.5')) {
        arr.push('half')
      }

      while (arr.length < LENGTH) {
        arr.push('off')
      }
      
      return arr
    },
    starSize: function () {
      return 'star-' + this.size
    }
  }
}
</script>
<style lang="stylus">
  @import "../../common/stylus/mixin.styl"
  .star
    text-align: center
    padding: 16px 0 28px
    .star-item
      width: 20px
      height: 20px
      display: inline-block
      background-size: 100% 100% 
      margin: 0 10px
      &.on
        bg-img(star48_on)
      &.half
        bg-img(star48_half)
      &.off
        bg-img(star48_off)
</style>
