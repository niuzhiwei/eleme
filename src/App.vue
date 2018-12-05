<template>
  <div id="app">
    <Header :seller="seller"></Header>
    <Tab></Tab>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import Header from './components/header/Header';
import Tab from './components/tab/Tab';
import { urlParse } from './common/js/util';

export default {
  name: 'App',
  components:{Header,Tab},
  data(){
    return{
      seller:{
        id:(()=>{
          let queryParam = urlParse()
          console.log(queryParam)
          return queryParam.id
        })()
      },
      goods:{},
      errOK:0
    }
  },
  methods:{
      getSeller(){
      this.axios.get('/api/seller'+'?id='+this.seller.id).then((res)=>{
        if(res.data.errno===this.errOK){
          this.seller = Object.assign({}, this.seller, res.data.data);
        }
      })
      }
    },
  created(){
     this.getSeller()
  }
  
}
</script>

<style>
</style>
