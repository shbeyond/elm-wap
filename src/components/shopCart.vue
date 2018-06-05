<template>
    <div class="shopCart">
        <div class="content">
            <div class="content-left" @click="listToggle" style="height:100%;">
                <div class="logo-wrapper">
                    <div class="badge" v-show="totalCount"><!-- 购物车图标右上角的数量提示-->
                        {{totalCount}}
                    </div>
                    <div class="logo" :class="{'active':totalCount>0}">
                        <i class="el-icon-goods"></i>
                    </div>
                </div>
                <div class="price" :class="{'active':totalPrice}">
                    ￥{{totalPrice}}
                </div>
                <div class="desc">
                    另需要配送费￥4元
                </div>
            </div>
            <div class="content-right" :class="{'enough':totalPrice>=20}">
                {{payDesc}}
            </div>
        </div>
        <transition name="transHeight">
            <div class="shopcartt-list" v-show="listShow">
                <div class="list-header">
                <h1 class="title">购物车</h1>
                <span class="empty" @click="setEmpty()">清空</span>
                </div>
                <div class="list-content" ref="foodlist">
                    <ul>
                        <li class="food" v-for="food in newSelectDate">
                        <span class="name">{{food.name}}</span>
                        <div class="price">
                            <span>￥{{food.price * food.count}}</span>
                        </div>
                        <div class="cartcontrol-wrapper">
                            <cartcontrol :foods="food"></cartcontrol>
                        </div>
                        </li>
                    </ul>
                </div>
            </div>
        </transition>
        <transition name="fade-backdrop">
            <div class="backdrop" v-show="showBackdrop" @click="hideBackdrop"></div>
        </transition>
    </div>
</template>

<script>
import Control from './cartcontrol.vue'
import BScroll from 'better-scroll'
export default {
    watch:{
        selectedFoods:(newVal,oldVal)=>{
            // console.log(newVal)
        }
    },
    data(){
        return{
            balls: 
            [
                {
                    show: false
                }, {
                    show: false
                }, {
                    show: false
                }, {
                    show: false
                }, {
                    show: false
                }
            ],
            dropBalls: [],
            listShow:false,
            newSelectDate:[]
        }
    },
    props:{
        selectedFoods:{
            type:Array,
            default:[]
        }
    },
    created(){
         
        // this.$root.eventHub.$on('cart.add', this.drop)

    },
    methods:{
        hideBackdrop(){
            this.listShow = false;
        },
        listToggle(){
            
            if (!this.selectedFoods.length) {
                return
            }
            this.listShow = !this.listShow
            if (this.listShow) {
                this.$nextTick(() => {
                if (!this.foodlistScroll) {
                    this._initScroll()
                } else {
                    this.foodlistScroll.refresh()
                }
                })
            }
        },
        setEmpty(){
             if(this.selectedFoods.length>0){
                  this.selectedFoods[0].foods.forEach((value)=>{
                        value.count = 0;
                    })
             }

        },
        _initScroll() {
            this.foodlistScroll = new BScroll(this.$refs.foodlist, {
                click: true
            });
        },
    
    },
    computed:{
    
    showBackdrop(){
        if (this.listShow && this.totalPrice) {
            return true
      }
      this.listShow = false
      return false
    },
    totalCount(){
        if(this.selectedFoods.length>0){
             let counts = 0;
            this.selectedFoods[0].foods.forEach((value)=>{
                if(value.count){
                    counts += value.count;
                }
            })
            return counts
        }
    },
    totalPrice(){
          let price = 0;
          if(this.selectedFoods.length>0){
              let newDate = this.selectedFoods[0].foods
           
            this.newSelectDate  = newDate.filter((val)=>{
                    if(val.count){
                        return val
                    }
                })
            
            this.selectedFoods[0].foods.forEach((value)=>{
                if(value.count){
                    price += value.count*value.price;
                }
            })
            
        }
        return price
    },
    payDesc(){
        let diff = 20 - this.totalPrice;
        
        if(diff<=0){
            return "去结算"
        }else{
            return `还差￥${diff}起送`                    
        }
    }
        
    },
    components:{
        'cartcontrol':Control
    }
}
</script>
<style scoped>


.backdrop{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(7,17,27,0.6);
   
    z-index: 40;
}
.shopCart .shopcartt-list .list-content .food .cartcontrol-wrapper{
    font-size: 14px;
    margin-top: 6px;
}
.shopCart .shopcartt-list .list-content .food .price{
    font-size: 14px;
    font-weight: 700;
    color: #f01414;
    padding: 0 12px 0 18px;
    line-height: 48px;
}
.shopCart .shopcartt-list .list-content .food .name{
    -ms-flex: 1;
    flex: 1;
    font-size: 14px;
    color: #07111b;
    line-height: 48px;
    font-weight: 700;
}
.shopCart .shopcartt-list .list-content .food{
    position: relative;
    display: -ms-flexbox;
    display: flex;
    height: 48px;
    margin: 0 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
}
.shopCart .shopcartt-list .list-content{
    max-height: 217px;
    overflow: hidden;
}
.shopCart .shopcartt-list .list-header .empty{
    position: absolute;
    right: 8px;
    font-size: 12px;
    color: #00a0dc;
    padding: 0 10px;
}
.shopCart .shopcartt-list .list-header .title{
    display: inline-block;
    font-size: 14px;
    font-weight: 200;
    color: #07111b;
    padding-left: 18px;
}
.shopCart .shopcartt-list .list-header{
    height: 40px;
    line-height: 40px;
    background: #f3f5f7;
    border-bottom: 1px solid rgba(7,17,27,0.1);
}
.shopCart .shopcartt-list{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    background: #fff;
    transform: translate3d(0, -100%, 0);
    z-index: 41;
}
    .logo-wrapper .logo.active{
         background: #00a0dc;
         color: #fff;
    }
    .logo{
        background: #80858a;
        color: #e1e2db;
        width: 100%;
        height: 100%;
        border-radius: 50%;
        font-size: 24px;
        line-height: 44px;
        font-weight: 700;
    }
    .shopCaring:before{
        content:"\E906";
    }
    .badge{
        position: absolute;
        top: 0;
        right: 0;
        background: #f01414;
        color: #fff;
        width: 24px;
        height: 16px;
        line-height: 16px;
        font-size: 9px;
        box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.4);
        font-weight: 700;
        border-radius: 16px;
        text-align: center;
    }
    .shopCart .content .content-right.enough{
        background: #00b43c;
        color: #fff;
    }
    .shopCart .content .content-right{
        -ms-flex: 0 0 105px;
        flex: 0 0 105px;
        font-size: 12px;
        font-weight: 700;
        background: #2b343c;
        color: rgba(255,255,255,0.4);
        line-height: 48px;
        text-align: center;
    }
    .shopCart .content .content-left .desc{
        position: relative;
        display: inline-block;
        vertical-align: top;
        margin: 12px 0 0 12px;
        font-size: 10px;
        color: rgba(255,255,255,0.4);
        font-weight: 700;
        line-height: 24px;
    }
    .shopCart .content .content-left .price.active{
        color: #fff;
    }
    .shopCart .content .content-left .price{
        display: inline-block;
        vertical-align: top;
        font-size: 16px;
        margin-top: 12px;
        padding-right: 12px;
        box-sizing: border-box;
        color: rgba(255,255,255,0.4);
        font-weight: 700;
        line-height: 24px;
        border-right: 1px solid rgba(255,255,255,0.1);
    }
    .logo-wrapper{
        display: inline-block;
        vertical-align: top;
        position: relative;
        height: 56px;
        line-height: 56px;
        border-radius: 50%;
        width: 56px;
        top: -10px;
        background: #141d27;
        margin: 0 12px;
        padding: 6px;
        box-sizing: border-box;
        text-align: center;
    }
    .shopCart{
        position: fixed;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 48px;
        z-index: 50;
    }
    .content{
        background: #141d27;
        display: flex;
        justify-content: space-between;
    }
</style>

