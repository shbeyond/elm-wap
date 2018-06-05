<template>
    <div>
        <div class="header">
            <div class="content-wrapper">
                <div class="avatar">
                    <img width="64" height="64" :src="resSeller.avatar">
                </div> 
                <div class="content">
                <div class="title">
                    <span class="brand">品牌</span> 
                    <span class="name">{{resSeller.name}}</span>
                </div> 
                <div class="description">
                {{resSeller.description}}/28分钟送达
                </div> 
                <div class="supports">
                    <div class="supports_desc">
                        <span class="icon decrease">减</span> 
                        <span class="text">在线支付满28减5</span>
                    </div>
                </div>
                    </div> 
                <div class="support-count">
                    <span class="count">5个</span> <i class="el-icon-arrow-right"></i>
                </div>
            </div>
            <div id="recommonce">
                <span class="gonggao">公告</span>
                <span class="space_overflow">{{resSeller.bulletin}}</span>
                <span class="el-icon-arrow-right" style="line-height:30px;"></span>
                
            </div>
            <div class="background">
                <img width="100%" height="100%" :src="resSeller.avatar">
            </div>
        </div>
    </div>
</template>

<script>

export default {
    data(){
      return{

      }
    },
    created(){
       
    },
    props:{
       resSeller:{
           type:Object
       }
    },
    methods:{

    }
}
</script>
<style scope>
    .header .background {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        filter: blur(10px);
        z-index: -1;
    }
    .gonggao{
        width: 25px;
        font-size: 8px;
        height: 20px;
        line-height: 30px;
        color: rgba(32, 233, 92, 0.678);
        
    }
    #recommonce{
        display: flex;
        justify-content: space-between;
    }
    .space_overflow{
        white-space: nowrap;
        font-size: 10px;
        vertical-align: middle;
        margin: 0 4px;
        line-height: 28px; 
        width: 80%;
        text-overflow:ellipsis;
        overflow: hidden;
    }
    *{
        margin: 0;
        padding: 0;
        border: 0;
        font-size: 100%;
        font-weight: normal;
        vertical-align: baseline;
    }
    .header{
        position: relative;
        background: rgba(7,17,27,0.5);
        color: #fff;
        blur: 10px;
        overflow: hidden;
    }
    .header .content-wrapper {
        position: relative;
        display: -ms-flexbox;
        display: flex;
        padding: 24px 12px 18px 24px;
        font-size: 12px;
    }
    .header .content-wrapper .avatar img {
        border-radius: 2px;
    }
    .header .content-wrapper .content {
        margin-left: 16px;
    }
    .header .content-wrapper .content .title {
        margin: 2px 0 8px 0;
        font-size: 16px;
    }
    .header .content-wrapper .content .title .brand {
   
        background: red;
        color:white;
        box-sizing: border-box;
        padding: 2px 5px;
    }
    .header .content-wrapper .content .title .name {
        margin-left: 6px;
        font-size: 16px;
        line-height: 18px;
        font-weight: bold;
    }
    .header .content-wrapper .content .description {
        font-size: 12px;
        margin-bottom: 10px;
    }
    .header .content-wrapper .content .supports .icon {
        padding:2px;
        background:#fff;
        color:red;
    }
    .header .content-wrapper .content .supports .text {
        line-height: 12px;
        font-size: 10px;
    }
    .header .content-wrapper .support-count {
        position: absolute;
        right: 12px;
        bottom: 18px;
        padding: 0 8px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background-color: rgba(0,0,0,0.2);
        text-align: center;
    }
    .header .content-wrapper .support-count .count {
        vertical-align: top;
        font-size: 10px;
    }
    .header .content-wrapper .support-count .icon-keyboard_arrow_right {
        font-size: 10px;
        margin-left: 2px;
        line-height: 24px;
    }
</style>

