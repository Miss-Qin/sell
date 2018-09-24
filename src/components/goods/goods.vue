<template>
    <div class="goods">
        <div class="menu-wrapper" ref="menuwrapper">
            <ul>
                <li v-for="(item, index) in goods" :key="index" class="menu-item" :class="{current:currentIndex === index}" @click="changeMenu(index)">
                    <span class="text border-1px">
                        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref="foodsWrapper">
            <ul>
                <li class="food-list food-list-hook" v-for="(item, index) in goods" :key="index">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li class="food-item border-1px" v-for="(itemSon, index) in item.foods" :key="index">
                            <div class="icon">
                                <img width="57" :src="itemSon.icon" alt="">
                            </div>
                            <div class="content">
                                <div class="name">{{itemSon.name}}</div>
                                <div class="desc">{{itemSon.description}}</div>
                                <div class="extra">
                                    <span class="count">月售{{itemSon.sellCount}}</span>
                                    <span>好评率{{itemSon.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">¥ {{itemSon.price}}</span>
                                    <span class="old" v-show="itemSon.oldPrice">¥ {{itemSon.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol :food="itemSon" @click="cart_add(event)"></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart :selected-foods="selectedFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    </div>
</template>

<script type="text/ecamscript-6">
import BScroll from "better-scroll";
import shopcart from "../shopcart/shopcart.vue";
import cartcontrol from "../cartcontrol/cartcontrol";
const ERR_OK = 0;

export default {
  name: "goods",
  props: {
    seller: Object
  },
  data() {
    return {
      classMap: [],
      goods: [],
      listHeight: [],
      scrollY: 0
    };
  },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i;
        }
      }
      return 0;
    },
    selectedFoods() {
      let foods = [];
      this.goods.forEach(good => {
        good.foods.forEach(food => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  created() {
    this.classMap = ["decrease", "discount", "guarantee", "invoice", "special"];
    this.$http.get("/api/goods").then(response => {
      if (response.body.errno === ERR_OK) {
        this.goods = response.body.goods;
        this.$nextTick(() => {
          this._initScroll();
          this._caculateHeight();
        });
      }
    });
  },
  methods: {
    _initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuwrapper, {
        click: true
      });
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType: 3
      });
      this.foodsScroll.on("scroll", pos => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _caculateHeight() {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName(
        "food-list-hook"
      );
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    changeMenu(index) {
      console.log(index);
      let foodList = this.$refs.foodsWrapper.getElementsByClassName(
        "food-list-hook"
      );
      let el = foodList[index];
      this.foodsScroll.scrollToElement(el, 200);
    },
    cart_add(target) {
      console.log(target);
    }
  },
  components: {
    shopcart,
    cartcontrol
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.goods {
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;

    .menu-wrapper {
        flex-basis: 80px;
        background: #f3f5f7;

        .menu-item {
            display: table;
            height: 54px;
            width: 56px;
            line-height: 14px;
            padding: 0 12px;

            &.current {
                position: relative;
                z-index: 10;
                margin-top: -1px;
                background-color: #fff;
                font-weight: 700;

                .text {
                    border-none();
                }
            }

            .text {
                display: table-cell;
                width: 56px;
                vertical-align: middle;
                border-1px(rgba(7, 17, 27, 0.1));
                font-size: 12px;

                .icon {
                    display: inline-block;
                    width: 12px;
                    height: 12px;
                    margin-right: 2px;
                    background-size: 12px 12px;
                    background-repeat: no-repeat;

                    &.decrease {
                        bg-img('decrease_3');
                    }

                    &.discount {
                        bg-img('discount_3');
                    }

                    &.guarantee {
                        bg-img('guarantee_3');
                    }

                    &.invoice {
                        bg-img('invoice_3');
                    }

                    &.special {
                        bg-img('special_3');
                    }
                }
            }
        }
    }

    .foods-wrapper {
        flex: 1;

        .title {
            height: 26px;
            line-height: 26px;
            padding-left: 14px;
            border-left: 2px solid #d9dde1;
            background-color: #f3f5f7;
            font-size: 12px;
            color: rgb(147, 153, 159);
        }

        .food-item {
            display: flex;
            margin: 0 18px;
            padding: 18px 0;
            border-1px(rgba(7, 17, 27, 0.1));

            .icon {
                flex-basis: 57px;
                margin-right: 10px;
            }

            .content {
                flex: 1;

                .name {
                    margin: 2px 0px 8px 0;
                    font-size: 14px;
                    line-height: 14px;
                    color: rgb(7, 17, 27);
                }

                .desc {
                    margin-bottom: 8px;
                    font-size: 10px;
                    line-height: 12px;
                    color: rgb(147, 153, 159);
                }

                .extra {
                    margin-bottom: 8px;
                    font-size: 10px;
                    line-height: 10px;
                    color: rgb(147, 153, 159);

                    .count {
                        margin-right: 12px;
                    }
                }

                .price {
                    .now {
                        margin-right: 8px;
                        font-size: 14px;
                        line-height: 24px;
                        font-weight: 700;
                        color: rgb(240, 20, 20);
                    }

                    .old {
                        text-decoration: line-through;
                        font-size: 10px;
                        line-height: 24px;
                        font-weight: 700;
                        color: rgb(147, 153, 159);
                    }
                }

                .cartcontrol-wrapper {
                    position: absolute;
                    right: 0;
                    bottom: 18px;
                }
            }
        }
    }
}
</style>
