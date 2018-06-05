<template>
   <div class="car_control">
       <transition name="fadeRotate">
           <div class="cart-decrease" v-show="foods.count>0" @click.stop.prevent="decreaseCart($event)">
                <span class="el-icon-remove inner"></span>
           </div>
       </transition>
       <div class="cart-count" v-show="foods.count>0">
            {{foods.count}}
        </div>
       <div class="cart-add" @click.stop.prevent="addCart($event)">
            <i class="el-icon-circle-plus"></i>
       </div>
   </div>
</template>
<script>
import Vue from 'vue'
export default {
    data(){
        return{

        }
    },
    props:{
        foods:Object
    },
    methods:{
       addCart(event){
            if (!event._constructed) {
                return
            }
            if (!this.foods.count) {
                Vue.set(this.foods, 'count', 0)
            }
            this.foods.count++;
            this.$root.eventHub.$emit('cart.add', event.target)
            // console.log(this.foods)
       },
       decreaseCart(event){
           if (!event._constructed || !this.foods.count) {
                return
            }
            this.foods.count--;
       }
    },
    created(){
        
    }
}
</script>
<style scoped>
    .cart-count {
        display: inline-block;
        vertical-align: top;
        font-size: 10px;
        color: #93999f;
        line-height: 24px;
        text-align: center;
        padding: 6px 0;
    }
    .inner{
        line-height: 24px;
        font-size: 24px;
        color: #00a0dc;
        transition: all 0.4s linear;
    }
    .cart-decrease {
        display: inline-block;
        padding: 6px;
        transition: all 0.4s linear;
    }
    .cart-add{
        display: inline-block;
        vertical-align: top;
        font-size: 24px;
        color: #00a0dc;
        line-height: 24px;
        padding: 6px;
    }
</style>
