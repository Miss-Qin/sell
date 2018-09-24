<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{highlight:totalCount>0}">
                        <i class="icon-shopping_cart" :class="{highlight:totalCount>0}"></i>
                    </div>
                    <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{highlight:totalPrice>0}">¥{{totalPrice}}</div>
                <div class="desc">另需配送费¥{{deliveryPrice}}元</div>
            </div>
            <div class="content-right">
                <div class="pay" :class="{lighlight:totalPrice>=minPrice}">
                    {{payDesc}}
                </div>
            </div>
        </div>
        <div class="ball-container">
            <div v-for="(ball, index) in balls" :key="index" v-show="ball.show" class="ball"></div>
        </div>
    </div>
</template>

<script>
export default {
  name: "shopcart",
  props: {
    selectedFoods: {
      type: Array,
      default: function() {
        return [];
      }
    },
    deliveryPrice: Number,
    minPrice: Number
  },
  data() {
    return {
      balls: [
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        }
      ]
    };
  },
  created() {},
  computed: {
    totalPrice() {
      let totalPrice = 0;
      this.selectedFoods.forEach(food => {
        totalPrice += food.price * food.count;
      });
      return totalPrice;
    },
    totalCount() {
      let totalCount = 0;
      this.selectedFoods.forEach(food => {
        totalCount += food.count;
      });
      return totalCount;
    },
    payDesc() {
      if (this.totalPrice === 0) {
        return `¥${this.minPrice}元起送`;
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice;
        return `还差${diff}元起送`;
      } else {
        return "去结算";
      }
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.shopcart {
    position: fixed;
    left: 0;
    bottom: 0;
    height: 48px;
    width: 100%;

    .content {
        display: flex;
        background: #141d27;
        font-size: 0;

        .content-left {
            flex: 1;

            .logo-wrapper {
                display: inline-block;
                position: relative;
                top: -10px;
                margin-left: 12px;
                padding: 6px;
                width: 56px;
                height: 56px;
                box-sizing: border-box;
                vertical-align: top;
                border-radius: 50%;
                background: #141d27;

                .logo {
                    width: 100%;
                    height: 100%;
                    text-align: center;
                    border-radius: 50%;
                    background: #2b343c;

                    &.highlight {
                        background: #00a0dc;
                    }

                    .icon-shopping_cart {
                        font-size: 24px;
                        line-height: 44px;
                        color: rgba(255, 255, 255, 0.4);

                        &.highlight {
                            color: #fff;
                        }
                    }
                }

                .num {
                    position: absolute;
                    top: 0;
                    right: 0;
                    width: 24px;
                    height: 16px;
                    line-height: 16px;
                    text-align: center;
                    border-radius: 16px;
                    font-size: 9px;
                    font-weight: 700;
                    color: #ffffff;
                    background: rgb(240, 20, 20);
                    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
                }
            }

            .price {
                display: inline-block;
                padding: 0 12px;
                margin: 12px 0px;
                box-sizing: border-box;
                line-height: 24px;
                border-right: 1px solid rgba(255, 255, 255, 0.1);
                font-size: 16px;
                font-weight: 700;
                color: rgba(255, 255, 255, 0.4);

                &.highlight {
                    color: #fff;
                }
            }

            .desc {
                display: inline-block;
                margin: 12px 0;
                padding: 0 12px;
                line-height: 24px;
                font-size: 14px;
                font-weight: 700;
                color: rgba(255, 255, 255, 0.4);
            }
        }

        .content-right {
            flex-basis: 105px;

            .pay {
                height: 48px;
                line-height: 48px;
                text-align: center;
                background: #2b333b;
                font-size: 12px;
                font-weight: 700;
                color: rgba(255, 255, 255, 0.4);

                &.lighlight {
                    color: #fff;
                    background: #00b43c;
                }
            }
        }
    }

    .ball-container {
        .ball {
            position: fixed;
            left: 32px;
            bottom: 22px;
            z-index: 200;
        }
    }
}
</style>
