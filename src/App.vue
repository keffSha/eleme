<template>
  <div>
    <v-header :seller='seller'></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link> 
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link> 
      </div>
      <div class="tab-item">
        <router-link to="seller">商家</router-link> 
      </div>
    </div>
    <keep-alive>  
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import Header from "@/components/header/header";
import axios from "axios";
import { urlParse } from "@/common/js/util";

const ERR_OK = 0;

export default {
  name: "App",
  components: {
    "v-header": Header
  },
  data: function() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse();
          return queryParam.id;
        })()
      }
    };
  },
  created() {
    axios.get("/api/seller?id="+this.seller.id).then(response => {
      if(response.data.errno === ERR_OK){
        this.seller = Object.assign({}, this.seller, response.data.data);
        // this.seller=response.data.data;
      }
    });
  }
};
</script>

<style lang="scss" scoped>
  @import './common/scss/base';
  @import './common/scss/mixin';
  .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    @include border-1px(rgba(7,17,27,0.1));
    .tab-item{
      flex: 1;
      text-align: center;
      & > a{
        display: block;
        font-size: 14px;
        color: rgb(77, 85, 93);
        &.active{
          color:rgb(240, 20, 20)
        }
      }
    }
  }
</style>
