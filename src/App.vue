<template>
  <div id="app">
    <v-head :resSeller="resData"></v-head>
     <div class="tab">
            <div class="tab-item">
                <router-link to="/goods">商品</router-link>
            </div>
            <div class="tab-item">
                <router-link to="/ratings">评论</router-link>
            </div>
            <div class="tab-item">
                <router-link to="/seller">商家</router-link>
            </div>
      </div>
    <router-view/>
  </div>
</template>

<script>

import axios from 'axios'
import { Data } from './statics/data.js'
import Heada from './components/header.vue'

export default {
  name: 'App',
  data(){
    return{
      resData:Data.seller
    }
  },
  created(){

  },
  methods:{

  },
  components:{
    'v-head':Heada
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
}
 .tab{
      display: flex;
      width: 100%;
      height: 40px;
      line-height: 40px;
      position: relative;
      border-bottom: 1px solid #ddd;
  }
.tab .tab-item{
      flex: 1;
      text-align: center;
}
.tab .tab-item a{
  text-decoration: none;
  color:black;
}
.tab .tab-item .active{
  color:red;
}
</style>
