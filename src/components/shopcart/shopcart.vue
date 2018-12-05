<!-- 购物车组件 -->
<template>
  <div>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}元</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop.prevent="pay">
        <div class="pay" :class="payClass">{{payDesc}}</div>
      </div>
    </div>
    <div class="shopcart-list" v-show="listShow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty" @click="empty">清空</span>
      </div>
      <div class="list-content" ref="listContent">
        <ul>
          <li class="food" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>￥{{food.price*food.count}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
  <div class="list-mask" v-show="listShow" @click="hideList">
  </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from '../cartcontrol/cartcontrol'
export default {
  components:{
   cartcontrol
  },
  data(){
     return {
       fold:true
     }
  },
  props:{
    deliveryPrice:{
      type:Number,
      default:0
    },
    minPrice:{
      type:Number,
      default:0
    },
    selectFoods:{
      type:Array,
      default(){
        return []
      }
    }
  },
  computed:{
    listShow(){
        if(!this.totalCount){
          this.fold = true
          return false
        }
        let show = !this.fold
        if(show){
          this.$nextTick(()=>{
            if(!this.scroll){
             this.scroll = new BScroll(this.$refs.listContent,{
              click:true
            })
            }else{
              this.scroll.refresh()
            }
          })
        }
        return show
    },
    payClass(){
      if(this.totalPrice<this.minPrice){
        return 'not-enough'
      }
      else{
        return 'enough'
      }
    },
    totalPrice(){
      let total = 0
      this.selectFoods.forEach((food)=>{
           total += food.price * food.count
      })
      return total
    },
    totalCount(){
      let count = 0
      this.selectFoods.forEach((food)=>{
           count += food.count
      })
      return count
    },
    payDesc(){
      if(this.totalPrice === 0){
          return `需￥${this.minPrice}元起送`
      }
      else if(this.totalPrice<this.minPrice){
        return  `还差￥${this.minPrice - this.totalPrice}元起送`
      }
      else{
        return `去结算`
      }
    }
  },
  methods:{
        pay(){
        if(this.totalPrice<this.minPrice){
          return 
        }
        else{
          window.alert(`支付￥${this.totalPrice}元`)
        }
    },
    hideList(){
       this.fold = true
    },
    empty(){
       this.selectFoods.forEach((food)=>{
            food.count = 0
       })
    },
    toggleList(){
        if(!this.totalCount){
          return
        }
        this.fold = !this.fold
    },
    drop(el){
      console.log(el)
    }
  }
}

</script>
<style lang='stylus' scoped>
@import "../../common/stylus/mixin"
.shopcart
  position fixed
  bottom 0
  left 0
  z-index 50
  width 100%
  height 48px
  .content
    display flex
    background-color #141d27
    .content-left
      flex 1
      font-size 0
      .logo-wrapper
        display inline-block
        position relative
        top -10px
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        box-sizing border-box
        vertical-align top
        border-radius 50%
        background-color #141d27
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 9px
          font-weight 700
          color #ffffff
          background-color rgb(240,20,20)
          box-shadow 0 4px 8px 0 rgba(0,0,0,0.4)
        .logo
          width 100%
          height 100%
          border-radius 50%
          background-color #2b343c
          text-align center
          &.highlight
            background:rgb(0,160,220)
          .icon-shopping_cart
            font-size 24px
            color #80858a
            line-height 44px
            &.highlight
              color #fff
      .price
        display inline-block
        vertical-align top
        line-height 24px
        margin-top 12px
        box-sizing border-box
        padding-right 12px
        border-right:1px solid rgba(255,255,255,0.1)
        font-size 16px
        font-weight 700
        color rgba(255,255,255,0.4)
        &.highlight
          color #fff
      .desc
        display inline-block
        vertical-align top
        line-height 24px
        margin 12px 0 0 12px
        font-size 10px
        color rgba(255,255,255,0.4)
    .content-right
      flex 0 0 105px
      width 105px
      .pay
        height 48px
        line-height 48px
        text-align center
        font-size 12px
        color rgba(255,255,255,0.4)
        font-weight 700
        background-color #2b333b
        &.not-enough
          background:#2b333b
        &.enough
          background #00b43c
          color #fff
  .ball-container
    .ball
      position fixed
      left 32px
      bottom 22px
      z-index 200
      &.drop-transition
        transition all 0.4s
        .inner
          width 16px
          height 16px
          border-radius 50%
          background-color rbg(0,160,220)
          transition all 0.4s
  .shopcart-list
    position absolute
    width 100%
    bottom 40px
    left 0
    z-index -1
    background-color #fff
    .list-header
      height 40px
      line-height 40px
      padding 0 18px
      background-color #f3f5f7
      border-bottom 1px solid rgba(7,17,27,0.1)
      .title
        float left
        font-size 14px
        color rgb(7,17,27)
      .empty
        float right
        font-size 12px
        color rgb(0,160,220)
    .list-content
      padding 0 18px
      max-height 217px
      overflow hidden
      .food
        position relative
        padding 12px 0
        box-sizing border-box
        border-1px(rgba(7,17,27,0.1))
        .name
          line-height 24px
          font-size 14px
          color rgb(7,17,27)
        .price
          position absolute
          right 90px
          bottom 12px
          line-height 24px
          font-size 14px
          font-weight 700
          color rgb(240,20,20)
        .cartcontrol-wrapper
          position absolute
          right  0
          bottom 6px
.list-mask
   position fixed
   top 0
   left 0
   width 100%
   height 100%
   z-index 40
   background rgba(7,17,27,0.6)
</style>