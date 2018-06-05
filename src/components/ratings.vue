<template>
      <div class="ratingsWrapper" ref="ratingsWrapper">
        <div class="ratings-content">
            <div class="info">
                <div class="mark">
                    <div class="num">{{seller.score}}</div>
                    <div class="text">综合评分</div>
                    <div class="contrast">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="stars">
                    <div class="serviceScore">
                        <span class="text">服务态度</span>
                        <el-rate
                            v-model="seller.serviceScore"
                            disabled
                            show-score
                            text-color="#ff9900"
                            score-template='4.5'>
                        </el-rate>
                        <!-- <span class="num">{{seller.serviceScore}}</span> -->
                    </div>
                    <div class="foodScore">
                        <span class="text">服务态度</span>
                         <el-rate
                            v-model="seller.serviceScore"
                            disabled
                            show-score
                            text-color="#ff9900"
                            score-template='4.5'>
                        </el-rate>
                        
                    </div>
                    <div class="deliveryTime">
                        <span class="text">送达时间</span>
                        <span class="time">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <div class="divider"></div>
            <div class="evaluation">
                <div class="classify">
                    <span v-for="(item,index) in classifyArr" class="item" :class="{'active':item.active,'bad':index==2,'badActive':item.active&&index==2}" @click="filterEvel(item)">
                        {{item.name}}<span class="count">{{item.count}}</span>
                    </span>
                </div>
            </div>

            <!-- 评价展示 -->
            <div class="evel-list">
                <ul>
                    <li class="evel" v-for="evel in evelArr">
                    <div class="avatar">
                        <img :src="evel.avatar" width="28" height="28">
                    </div>
                    <div class="content">
                        <div class="user">
                            <span class="name">{{evel.username}}</span>
                            <span class="rateTime">{{evel.rateTime}}</span>
                        </div>
                        <div class="star-wrapper">
                        <!-- <star :size="24" :score="evel.score"></star> -->
                         <el-rate
                            v-model="seller.score"
                            disabled
                            show-score
                            text-color="#ff9900">
                        </el-rate>
                            <span class="deliveryTime">{{evel.deliveryTime}}分钟送达</span>
                        </div>
                        <div class="text">
                        {{evel.text}}
                        </div>
                        <div class="recommend">
                            <span class="icon icon-thumb_up" v-show="evel.recommend.length"></span>
                            <span class="dish" v-for="dish in evel.recommend">{{dish}}</span>
                        </div>
                    </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
            
</template>

<script>
import { Data } from "../statics/data.js";
import BScroll from 'better-scroll'


 

export default {
  data() {
    return {
      seller: {},
      classifyArr: [
        {
          name: "全部",
          count: 0,
          active: true
        },
        {
          name: "推荐",
          count: 0,
          active: false
        },
        {
          name: "吐槽",
          count: 0,
          active: false
        }
      ],
      evelflag:true,
      ratings:[]
    };
  },
  created() {
    this.initData();
  },
  methods: {
    initData() {
      this.seller = Data.seller;
      this.ratings = Data.ratings;
      this._initClassifyArr();
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.ratingsWrapper, {
            click: true
          })
        })
    },
    filterEvel(item) {
      this.classifyArr.forEach((data) => {
        data.active = false
      })
      item.active = true
    },
    _initClassifyArr() {
      this.classifyArr.forEach((data, index) => {
        if (index) {
          data.count = this.ratings.filter((temp) => temp.rateType === index - 1).length
        } else {
          data.count = this.ratings.length
        }
      })
    },
  },
  computed:{
      evelArr(){
          let selectIndex = 0;
          this.classifyArr.forEach((data,index)=>{
              if(data.active){
                  selectIndex = index;
              }
          })
          if (this.scroll) {
            this.$nextTick(() => {
            this.scroll.refresh()
            })
        }
         return selectIndex ? this.ratings.filter((data) => this.evelflag ? data.rateType === selectIndex - 1 && data.text : data.rateType === selectIndex - 1) : this.ratings.filter((data) => this.evelflag ? data.text : true)
      }
  }
};
</script>
<style scope>
.ratings-content .evel-list .evel .content .recommend .dish{
    display: inline-block;
    font-size: 9px;
    color: #93999f;
    line-height: 16px;
    border: 1px solid rgba(7,17,27,0.1);
    padding: 2px 6px;
    margin-right: 8px;
    white-space: normal;
    margin-top: 4px;
}
.ratings-content .evel-list .evel .content .recommend .icon{
    font-size: 12px;
    color: #00a0dc;
    line-height: 16px;
}
.ratings-content .evel-list .evel .content .recommend{
    padding-top: 4px;
}
.ratings-content .evel-list .evel .content .text{
    font-size: 12px;
    color: #07111b;
    line-height: 18px;
}
.ratings-content .evel-list .evel .content .star-wrapper .deliveryTime{
    font-size: 10px;
    padding-left: 6px;
    font-weight: 200;
    color: #93999f;
}
.ratings-content .evel-list .evel .content .star-wrapper .star{
    display: inline-block;
}
.ratings-content .evel-list .evel .content .star-wrapper{
    font-size: 0;
    padding-top: 4px;
    margin-bottom: 6px;
}
.ratings-content .evel-list .evel .content .user .rateTime{
    position: absolute;
    font-weight: 200;
    right: 18px;
    color: #93999f;
}
.ratings-content .evel-list .evel .content .user{
    font-size: 10px;
    color: #07111b;
    line-height: 12px;
}
.ratings-content .evel-list .evel .avatar img{
    border-radius: 50%;
}
.ratings-content .evel-list .evel .avatar{
    -ms-flex: 0 0 28px;
    flex: 0 0 28px;
    margin-right: 12px;
}
.ratings-content .evel-list .evel{
    display: -ms-flexbox;
    display: flex;
    padding: 18px 0;
    margin: 0 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
}
.ratings-content .evaluation {
  padding: 18px 0;
  position: relative;
}
.ratings-content .evaluation .classify .item.bad {
  background: rgba(77, 85, 93, 0.2);
}
.ratings-content .evaluation .classify .item .count {
  font-size: 8px;
  padding-left: 2px;
}
.ratings-content .evaluation .classify .item.active {
  color: #fff;
  background: #00a9dc;
}

.ratings-content .evaluation .classify .item {
  display: inline-block;
  font-size: 12px;
  padding: 8px 12px;
  line-height: 16px;
  background: rgba(0, 160, 220, 0.2);
  color: #4d555f;
  margin-right: 8px;
}
.ratings-content .evaluation .classify {
  padding-bottom: 18px;
  margin: 0 18px;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.divider {
  height: 16px;
  width: 100%;
  background: #f3f5f7;
  border-top: 1px solid rgba(7, 17, 27, 0.1);
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}

.el-rate__item {
  display: inline-block;
  background-repeat: no-repeat;
  width: 15px;
  height: 15px;
  margin-right: 6px;
  background-size: 100% 100%;
}
.info {
  display: flex;
}
.info .mark {
  flex: 0 0 138px;
  margin: 18px 0;
  border-right: 1px solid rgba(7, 17, 27, 0.1);
  text-align: center;
}
.info .mark .num {
  font-size: 24px;
  color: #f90;
  line-height: 28px;
}
.info .mark .text {
  padding: 6px 0 8px 0;
  font-size: 12px;
  color: #07111b;
  line-height: 12px;
}
.info .mark .contrast {
  font-size: 10px;
  color: #07111b;
  line-height: 10px;
  margin-bottom: 6px;
}
.info .stars {
  padding: 18px 24px;
}
.info .serviceScore,
.deliveryTime,
.foodScore {
  display: flex;
  margin-bottom: 8px;
}
.info .text {
  font-size: 12px;
  color: #07111b;
  line-height: 18px;
  margin-right: 12px;
}
</style>

