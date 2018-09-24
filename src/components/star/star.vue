<template>
    <div class="star" :class="starType">
        <span v-for="(item, index) in isStar" :key="index" :class="item" class="star-item"></span>
     </div>
</template>

<script>
const LENGTH = 5;
const CLS_ON = "on";
const CLS_HALF = "half";
const CLS_OFF = "off";
export default {
  props: {
    size: Number,
    scroe: Number
  },
  computed: {
    starType () {
      return "star-" + this.size;
    },
    isStar () {
      let res = [];
      let scroe = Math.floor(this.scroe * 2) / 2;
      let hasDecimal = scroe % 1 !== 0;
      let integer = Math.floor(scroe);
      for (let i = 0; i < integer; i++) {
        res.push(CLS_ON);
      }
      if (hasDecimal) {
        res.push(CLS_HALF);
      }
      while (res.length < LENGTH) {
        res.push(CLS_OFF);
      }
      return res;
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.star {
    font-size: 0;

    .star-item {
        display: inline-block;
        background-repeat: no-repeat;
    }

    &.star-48 {
        .star-item {
            width: 20px;
            height: 20px;
            margin-right: 22px;
            background-size: 20px 20px;

            &:last-child {
                margin-right: 0;
            }

            &.on {
                bg-img('star48_on');
            }

            &.half {
                bg-img('star48_half');
            }

            &.off {
                bg-img('star48_off');
            }
        }
    }

    &.star-36 {
        .star-item {
            width: 15px;
            height: 15px;
            margin-right: 6px;
            background-size: 15px 15px;

            &:last-child {
                margin-right: 0;
            }

            &.on {
                bg-img('star36_on');
            }

            &.half {
                bg-img('star36_half');
            }

            &.off {
                bg-img('star36_off');
            }
        }
    }

    &.star-24 {
        .star-item {
            width: 10px;
            height: 10px;
            margin-right: 3px;
            background-size: 10px 10px;

            &:last-child {
                margin-right: 0;
            }

            &.on {
                bg-img('star24_on');
            }

            &.half {
                bg-img('star24_half');
            }

            &.off {
                bg-img('star24_off');
            }
        }
    }
}
</style>
