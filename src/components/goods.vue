<template>
    <div id="mainFood">
        <div class="goods" style="width:20%;background:#ccc;" ref="menuWrapper">
            <ul>
                <li v-for="(item,key) in menuData" @click="menuClick(key,$event)" :class="key==menuCompute?'menu-item-selected':'menu-item'">
                    <span class = "sideMenu">
                        <span class="text">
                            {{ item.name }}
                        </span>
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" id="wrapper" ref="foodsWrapper" style="width:80%;">
            <ul>
                <li v-for="item in menuData" class="food-list food-list-hook">
                <h1>{{item.name}}</h1>
                <ul>
                    <li v-for="food in item.foods" class="food-item" @click="goDetail(food)">
                    <div class="icon">
                        <img width="57" height="57" :src="food.icon"/>
                    </div>
                    <div class="content">
                        <h2>{{food.name}}</h2>
                            <p class="description" v-show="food.description">{{food.description}}</p>
                        <div class="sell-info" style="font-size:50%;">
                            <span class="sellCount" style="margin-right:4px;">月售{{food.sellCount}}份</span>
                            <span class="rating">好评率{{food.rating}}%</span>
                        </div>
                        <div class="price">
                            <span class="newPrice" style="font-size: 14px;color: #f01414;"><span class="unit">￥</span>{{food.price}}</span>
                            <span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
                        </div>
                        <div class="cartcontrol-wrapper">
                            <cartcontrol :foods="food"></cartcontrol>
                        </div>
                    </div>
                    </li>
                </ul>
                </li>
            </ul>
        </div>
        <v-shopcar :selectedFoods="selectfoods"></v-shopcar>
        <food-detail ref="myData" :thisSelectFood="fooddetailData"></food-detail>
    </div>
</template>
<script>
import { Data } from '../statics/data.js'
import BScroll from 'better-scroll'
import ShopCar from './shopCart.vue'
import CartControl from './cartcontrol.vue'
import Food_detail from './fooddetail.vue'
import axios from 'axios'
console.log(axios)

export default {
    data(){
        return{
            menuData:[],
            foodsScrollY:0,
            listHeight:[],
            fooddetailData:{}
        }
    },
    created(){

        this.menuData = Data.goods
        this.$nextTick(()=>{
            this.initScroll()//初始化scroll
            this.initHeight()//初始化列表高度
        })
        
    },
    computed:{
        menuCompute(){
            for (let i = 0, L = this.listHeight.length; i < L; i++) {
                let topHeight = this.listHeight[i]
                let bottomHeight = this.listHeight[i + 1]
                if (!bottomHeight || (this.foodsScrollY >= topHeight && this.foodsScrollY < bottomHeight)) {
                    return i
                }
            }
                return 0
        },
        selectfoods(){
            
            let FOOD = [];
            this.menuData.forEach((items)=>{
                // console.log(items.foods)
                items.foods.forEach((value)=>{
                    if(value.count){
                        FOOD.push(items)
                    }
                })
            })
            return FOOD
        }
    },
    methods:{
        initScroll(){
            this.menuWrapper = new BScroll(this.$refs.menuWrapper,{
                click:true
            })
            this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
                click:true,
                probeType:3
            })
            //监控滚动事件

            this.foodsScroll.on('scroll', (pos) => {
                this.foodsScrollY = Math.abs(Math.round(pos.y))
            })
        },
        initHeight(){
            let listWrapper = this.$refs.foodsWrapper.querySelectorAll('.food-list-hook');
            let height = 0;
            this.listHeight.push(height)
            for(let i=0,L=listWrapper.length;i<L;i++){
                let item  = listWrapper[i];
                height += item.clientHeight
                this.listHeight.push(height)
            }

        },
        menuClick(index,event){

            if(!event._constructed){
                return
            }
            this.foodsScroll.scrollTo(0, -this.listHeight[index], 300)
        },
        goDetail(food){
            this.fooddetailData = food
            this.$nextTick(()=>{
                this.$refs.myData.showToggle()
            })
        }
    },
    components:{
        'v-shopcar':ShopCar,
        'cartcontrol':CartControl,
        'food-detail':Food_detail
    }

}
</script>

<style scoped>
    .menu-item-selected{
        position: relative;
        display: table;
        height: 54px;
        line-height: 14px;
        width: 56px;
        padding: 0 12px;
        background: #fff;
        margin-top: -1px;
    }
    #mainFood{
        display: flex;
        position: absolute;
        top: 186px;
        bottom: 46px;
        width: 100%;
        overflow: hidden;
    }
    .cartcontrol-wrapper {
        position: absolute;
        right: 0;
        bottom: 12px;
        z-index: 20;
    }
    .oldPrice {
        text-decoration: line-through;
        color: #93999f;
        padding-left: 4px;
    }
    .foods-wrapper .food-item .content .price {
        font-size: 10px;
        font-weight: 700;
        line-height: 24px;
    }
    .foods-wrapper .food-item .content .description {
        font-size: 10px;
        color: #93999f;
        line-height: 10px;
    }
    .foods-wrapper .food-item .content .description {
        font-size: 10px;
        margin-bottom: 8px;
        line-height: 12px;
    }
    .foods-wrapper .food-item .content h2 {

        margin: 2px 0 8px 0;
        font-size: 14px;
        line-height: 14px;
        height: 14px;
        font-weight: 700;
        color: #07111b;
    }
    .foods-wrapper .food-item .content {
        flex: 1;
        padding-left: 10px;
    }
    #wrapper .food-item{

        position: relative;
        display: -ms-flexbox;
        display: flex;
        margin: 0 18px;
        padding: 18px 0;
        border-bottom: 1px solid rgba(7,17,27,0.1);
    }
    #wrapper h1{

        height: 26px;
        line-height: 26px;
        padding-left: 12px;
        font-size: 12px;
        color: #93999f;
        background: #f3f5f7;
        border-left: 2px solid #d9dde1;
    }
    li{
        list-style: none;
    }
    .menu-item{
        position: relative;
        display: table;
        height: 54px;
        line-height: 14px;
        width: 56px;
        padding: 0 12px;
    }
    .sideMenu{
        display: table-cell;
        vertical-align: middle;
        font-size: 12px;
        font-weight: 200;
        white-space: normal;
        line-height: 14px;
    }
    .text{
        vertical-align: middle;
    }
</style>

