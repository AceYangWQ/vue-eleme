<template>
  <div class="cartControl">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>

    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart">
    </div>
  </div>
</template>

<script>
  import Vue from 'vue';
  export default {
    props: {
     food: {
         type: Object
     }
    },
    created() {
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
            return;
        }
        if (!this.food.count) {
            Vue.set(this.food, 'count', 1);
        } else {
           this.food.count++;
        }
        // 子组件与父组件通信
        // 那么，如果子组件想要改变数据呢？这在vue中是不允许的，因为vue只允许单向数据传递，
        // 这时候我们可以通过触发事件来通知父组件改变数据，从而达到改变子组件数据的目的.
         this.$emit('add', event.target); // 主动触发 add 方法，'event.target' 为向父组件传递的数据
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
 };
</script>

<style lang="stylus" type="text/stylus">
  .cartControl
    font-size 0
    .cart-decrease
      display inline-block
      padding 6px
      transition all .4s linear
      &.move-enter-active
        opcity 1
        transform translate3d(0,0,0)
      .inner
        display inline-block
        line-height 24px
        font-size 24px
        color rgb(0,160,220)
        transition all .4s linear
        transform rotate(0)
      &.move-enter, &.move-leave-to
        opacity 0
        transform translate3d(24px,0,0)
        .inner
          transform rotate(180deg)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      font-size 10px
      color rgb(147,153,159)
    .cart-add
      display inline-block
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0,160,220)
</style>
