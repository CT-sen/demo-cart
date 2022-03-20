<template>
  <div class="goods-container">
      <div class="thumb">
          <div class="custom-control custom-checkbox">
              <input type="checkbox" 
              class="custom-control-input" 
              :id="'cb1'+id" 
              :checked="state"
              @change="stateChange"
             >
            <label class="custom-control-lable" :for="'cb1'+id">
                <img :src="pic">
            </label>
          </div>
      </div>
      <div class="goods-info">
          <h6 class="goods-title">{{title}}</h6>
          <div class="goods-info-bottom">
              <span class="goods-price">¥{{price}} </span>
          </div>
      </div>
      <Counter :num="count" :id="id"></Counter>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter.vue'
export default {
    props:{
        title:{
            default:'',
            type:String
        },
        pic:{
            default:'',
            type:String
        },
        price:{
            default:0,
            type:Number
        },
        state:{
            default:true,
            type:Boolean
        },
        id:{
            required:true,
            type:Number
        },
        count:{
            default:0,
            type:Number
        }

    },
    methods:{
        stateChange(e){
            // console.log(e);
            const newState = e.target.checked
        this.$emit('state-change',{id:this.id,value:newState})
        },
    },
    components:{
      Counter
      
    }
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
