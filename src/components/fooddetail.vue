<template>
<transition name="move">
    <div class="detailWrapper" ref="detailWrapper" v-if="showDetail">
        <div class="foodDetail">
            <div class="back" @click="showToggle()">
                <i class="el-icon-arrow-left"></i>
            </div>
            <img :src="thisSelectFood.image" height="425" width="100%">
            <div class="deinfo">
                <div class="detitle">{{thisSelectFood.name}}</div>
                <div class="dedesc">
                    <span>月售{{thisSelectFood.sellCount}}</span>
                    <span>好评率{{thisSelectFood.rating}}%</span>
                </div>
                <div class="deprice">
                    <span class="deunit">￥</span>{{thisSelectFood.price}}
                    <span class="deoldPrice" v-show="thisSelectFood.oldPrice">￥{{thisSelectFood.oldPrice}}</span>
                </div>
                <div class="deshopCart">
                    <transition name="fade">
                        <div class="detext" @click="addCart2($event)" v-show="!thisSelectFood.count">加入购物车</div>
                    </transition>
                </div>
                <cartcontrol :foods="thisSelectFood"></cartcontrol>
            </div>
            <div class="dividers"></div>
            <div class="nedesc">
            <div class="netitle">商品介绍</div>
                <div class="necontent">{{thisSelectFood.info}}</div>
            </div>
        </div>
    </div>
</transition>
    
</template>
<script>
import BScroll from 'better-scroll'
import Cartcontrol from './cartcontrol.vue'

export default {
    data(){
        return{
            showDetail:false
        }
    },
    props:{
        thisSelectFood:Object
    },
    methods:{
        addCart2(event){
            
            if (!event._constructed) {
                this._initScroll()
            }

            this.$set(this.thisSelectFood, 'count', 1)
            this.$root.eventHub.$emit('cart.add', event.target)
        },
        showToggle(){
            this.showDetail = !this.showDetail
            if (this.showDetail) {
                this.$nextTick(() => {
                    this._initScroll()
                })
            }
        },
        _initScroll(){
            if (!this.detailWrapper) {
            this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
                click: true
            });
            } else {
                this.detailWrapper.refresh()
            }
        }
    },
    components:{
        'cartcontrol':Cartcontrol
    }
        
    
}
</script>
<style scoped>
.nedesc{
    padding:20px;
}
.detailWrapper{
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    width: 100%;
    background: #fff;
    transition: all 0.4s ease;
    padding-bottom: 20px;
}
.necontent{
        font-size: 12px;
    font-weight: 200;
    color: #4d555d;
    line-height: 24px;
    padding: 0 8px;
}
.netitle{
    font-size: 14px;
    font-weight: 500;
    color: #07111b;
    margin-bottom: 6px;
}
.dividers{
        height: 16px;
    width: 100%;
    background: #f3f5f7;
    border-top: 1px solid rgba(7, 17, 27, 0.1);
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.foodDetail .deinfo .car_control{
    position: absolute;
    right: 12px;
    bottom: 12px;
}
.foodDetail .back{
    position: absolute;
    color: #fff;
    top: 12px;
    left: 6px;
    font-size: 30px;
    padding: 10px;
}
.cartcontrol .cart-add {
    display: inline-block;
    vertical-align: top;
    font-size: 24px;
    color: #00a0dc;
    line-height: 24px;
    padding: 6px;
}
.cartcontrol .cart-count {
    display: inline-block;
    vertical-align: top;
    font-size: 10px;
    color: #93999f;
    line-height: 24px;
    text-align: center;
    padding: 6px 0;
}
.cartcontrol .cart-decrease {
    display: inline-block;
    padding: 6px;
    transition: all 0.4s linear;
}
.foodDetail .deinfo .deshopCart .detext{
    box-sizing: border-box;
    height: 100%;
    line-height: 24px;
    color: #fff;
    font-size: 10px;
    padding: 0 12px;
    border-radius: 12px;
    background: #00a0dc;
}
.foodDetail .deinfo .deprice .deoldPrice{
    padding-left: 12px;
    font-size: 10px;
    font-weight: normal;
    color: #93999f;
    line-height: 24px;
    display: none;
}
.foodDetail .deinfo .deprice .deunit{
    font-size: 10px;
    font-weight: normal;
}
.foodDetail .deinfo .cartcontrol{
    position: absolute;
    right: 12px;
    bottom: 12px;
}
.foodDetail .deinfo .deshopCart{
    position: absolute;
    right: 18px;
    bottom: 18px;
    height: 24px;
    text-align: center;
    z-index: 2;
}
.foodDetail .deinfo .deprice{
    display: -ms-flexbox;
    display: flex;
    padding-top: 18px;
    font-size: 14px;
    font-weight: 700;
    color: #f01414;
    line-height: 24px;
}
.foodDetail .deinfo .dedesc{
    display: -ms-flexbox;
    display: flex;
    padding: 0;
    padding-top: 8px;
    font-size: 10px;
    color: #93999f;
    line-height: 10px;
}
.foodDetail .deinfo .detitle{
    font-size: 14px;
    font-weight: 700;
    color: #07111b;
    line-height: 14px;
}
   .foodDetail .deinfo{

        position: relative;
        box-sizing: border-box;
        width: 100%;
        padding: 18px;
    }
    .foodDetail{
        position: fixed;
        left: 0;
        top: 0;
        bottom: 48px;
        width: 100%;
        background: #fff;
        transition: all 0.4s ease;
    }
</style>

