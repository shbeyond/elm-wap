<template>
    <div>
        <h1>商家信息</h1>
        <ul class="info-list">
          <li v-for="info in seller.infos" class="ulli">{{info}}</li>
        </ul>
    </div>
</template>
<script>
import { Data } from '../statics/data.js'
export default {
    data(){
        return{
            seller:Data.seller
        }
    }
}
</script>
<style scoped>
.ulli{
    font-size: 12px;
    font-weight: 200;
    color: #07111b;
    line-height: 16px;
    padding: 16px 12px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    margin:0 18px;
    list-style: none;
} 
h1{
    margin: 0 18px;
    padding: 18px 0 12px 0;
    border-bottom: 1px solid rgba(7,17,27,0.1);
}
</style>