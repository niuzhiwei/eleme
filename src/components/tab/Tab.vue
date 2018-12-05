<!-- Vue组件 -->
<template>
  <div class="tab">
     <router-link to="/goods" tag="div" class="tab-item">商品</router-link>
     <router-link to="/ratings" tag="div" class="tab-item">评论</router-link>
     <router-link to="/sellers" tag="div" class="tab-item">商家</router-link>
  </div>
</template>

<script>
export default {

}

</script>
<style scoped lang="stylus">
@import '../../common/stylus/mixin.styl'
.tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px
    border-1px(rgba(7,17,27,0.1))
}
.tab-item{
    flex:1;
    text-align: center;
    font-size: 14px;
    color:rgb(77,85,93);
}
.router-link-active{
    color:rgb(240,20,20)
}
</style>