<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart" transition="move"></div>
        </transition>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click="addCart"></div>
    </div>
</template>

<script>
import Vue from "vue";
export default {
  props: {
    food: Object
  },
  created() {}, 
  methods: {
    addCart() {
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
      this.$emit("cart_add", event.target);
    },
    decreaseCart() {
      this.food.count--;
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cartcontrol {
    font-size: 0;

    .move-enter-active {
        transition: all 0.3s linear;
    }

    .move-leave-active {
        transition: all 0.3s cubic-bezier(0, 0, 1, 1);
    }

    .move-enter, .move-leave-to {
        transform: translate3d(24px, 0, 0);
        opacity: 0;
    }

    .cart-decrease {
        display: inline-block;
        padding: 6px;
        line-height: 24px;
        font-size: 24px;
        color: rgb(0, 160, 220);
    }

    .cart-count {
        display: inline-block;
        vertical-align: top;
        width: 12px;
        padding-top: 6px;
        font-size: 10px;
        line-height: 24px;
        text-align: center;
        color: rgb(147, 153, 159);
    }

    .cart-add {
        display: inline-block;
        padding: 6px;
        line-height: 24px;
        font-size: 24px;
        color: rgb(0, 160, 220);
    }
}
</style>
