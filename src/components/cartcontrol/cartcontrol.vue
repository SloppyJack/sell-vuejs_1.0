<template>
  <div class="cartcontrol">
    <div class="cart-decrease"
    v-show="food.count>0" @click.stop.prevent="decreaseCart" transition="move">
      <div class="inner icon-remove_circle_outline"></div>
    </div>
    <div class="cart-count" v-if="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        // 如果为浏览器原生点击事件,则直接返回
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          // vue无法对没有定义的对象进行getter和setter，所以需手动设置属性
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        // 派发事件，沿着父链冒泡
        this.$dispatch('cart.add', event.target);
      },
      decreaseCart(event) {
        // 如果为浏览器原生点击事件,则直接返回
        if (!event._constructed) {
          return;
        }
        if (this.food.count > 0) {
          this.food.count--;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-decrease
      display inline-block
      padding 6px
      transition all 0.4s linear
      &.move-transition
        opacity 1
        transform translate3d(0, 0, 0)
        .inner
          display inline-block
          line-height 24px
          font-size 24px
          color rgb(0, 160, 220)
          transition all 0.4s linear
          transform rotate(0)
      &.move-enter, &.move-leave
        opacity 0
        transform translate3d(24px, 0, 0)
        .inner
          transform rotate(180deg)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      text-align center
      font-size 10px
      color rgb(147, 153, 159)
    .cart-add
      display inline-block
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0, 160, 220)
</style>
