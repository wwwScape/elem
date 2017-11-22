<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{hightlight:totalCount>0}">
            <span class="icon-shopping_cart"></span>
          </div>
          <div :class="{num:totalCount>0}">{{totalCount}}</div>
        </div>
        <div class="price" :class="{hightlight:totalCount>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">{{payDesc}}</div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      selectFoods: {
        type: Array,
        default(){
          return [
            {
              price: 10,
              count: 0
            }
          ];
        }
      }
    },
    // 计算属性
    computed: {
      // 计算购买商品的总价
      totalPrice(){
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      // 所选商品的总和
      totalCount(){
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      // 计算还差多少钱起送
      payDesc(){
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`
        } else {
          return '去结算';
        }
      },
      // 去结算的class样式
      payClass(){
        if (this.totalPrice < this.minPrice) {
            return 'not-enough'
        }else {
            return 'enough'
        }
      }

    }
  }
</script>

<style lang="stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width: 100%
    height: 48px
    .content
      display: flex
      background: #141d27
      .content-left
        flex: 1
        font-size: 0
        .logo-wrapper
          display: inline-block
          position: relative
          top: -10px
          margin: 0 12px
          padding: 6px
          width: 56px
          height: 56px
          box-sizing: border-box
          vertical-align: top
          border-radius: 50%
          background: #141d27
          .logo
            width: 100%
            height: 100%
            border-radius: 50%
            background: #2b343c
            text-align: center
            .icon-shopping_cart
              margin: 0 auto
              line-height: 44px
              font-size: 24px
              color: rgba(255, 255, 255, 0.4)
            &.hightlight
              background: rgb(0, 160, 220)
              .icon-shopping_cart
                color: #fff
          .num
            position: absolute
            top: 0
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align: center
            border-radius: 16px
            font-size: 9px
            color: #fff
            background: rgb(240, 20, 20)
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price, .desc
          display: inline-block
          vertical-align: top
          line-height: 24px
          margin-top: 12px
          padding-right: 12px
          border-sizing: border-box
          color: rgba(255, 255, 255, 0.4)
        .price
          font-size: 16px
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          font-weight: 700
          &.hightlight
            color: #fff
        .desc
          display: inline-block
          margin: 12px 0 0 12px
          font-size: 10px
      .content-right
        flex: 0 0 105px
        width: 105px
        .pay
          height: 48px
          line-height: 48px
          text-align: center
          font-size: 12px
          color: rgba(255, 255, 255, 0.4)
          font-weight: 700
          &.not-enough
            background: #2b333b
          &.enough
            background: #00b43c
            color: #fff

</style>