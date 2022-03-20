<template>
  <div class="app-container">
    <!-- Header头部区 -->
    <Header title="购物车案例"></Header>
    <!-- Goods商品区 -->
    <Goods v-for="item in list" 
      :key="item.id" 
      :title="item.goods_name" 
      :pic="item.goods_img" 
      :price="item.goods_price" 
      :state="item.goods_state" 
      :id="item.id"
      :count="item.goods_count"
       @state-change='getNewState'>
    </Goods>
    <!-- Footer结算区 -->
    <Footer :isfull="fullState"
    :amount = 'awt'
    :all="total"
    @fullChange = 'getFullState'
   ></Footer>
  </div>
</template>

<script>
import axios from 'axios'
import bus from '@/components/eventBus.js'
import Header from '@/components/Hander/Hander.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'

export default {
  data(){
    return {
      list:[]
    }
  },
  
    computed:{
        fullState(){

            return this.list.every(item=>item.goods_state)
        },
        awt(){
          return this.list
          .filter(item =>item.goods_state)
          .reduce((total,item) => (total += item.goods_price*item.goods_count),0)
        },
        total(){
         return this.list.filter(item =>item.goods_state).reduce((t,item)=>(t += item.goods_count),0)
        }
    },
  created(){
    this.initCartList(),
    bus.$on('share',val => {
      // console.log('app接受到了值',val);
      this.list.some(item =>{
        if(item.id === val.id){
          item.goods_count = val.value
          return true
        }
      })
    })
  },
  methods:{
    async initCartList(){
    const {data:res} = await axios.get('https://www.escook.cn/api/cart')
    console.log(res)
    if(res.status === 200){
      this.list = res.list
    }
    },
    getNewState(e){
      // console.log(e);
     this.list.some(item =>{
       if(item.id === e.id) {
         item.goods_state = e.value
       }
     })
    },
    getFullState(val){
      // console.log('1');
      this.list.forEach(item =>item.goods_state = val)
    }
  },
  components: {
    Header,
    Goods,
    Footer
  },
  
}
</script>


<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
