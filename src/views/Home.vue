<template>
  <div class="home">
    <van-nav-bar>
      <template #left>
       <div class="home-nav">
           <div class="t1">{{ times }}</div>
            <div class="t2">zy</div>
  </div>
</template>
         <template #right>
             <div class="home-search">
              <van-search shape="round" placeholder="请输入搜索关键词"/></div>
              </template>
              </van-nav-bar>
              <!--商品内容-->
              <div class="home-content">
                <!--轮播图-->
                <div class="banner-box">
                <van-swipe :autoplay="3000">
                <van-swipe-item
                v-for="(item,index) in bannerData"
                :key="index"
                >
                <img class="auto_img" :src="item.bannerImg" alt="" />
                </van-swipe-item>
              </van-swipe>
              
              </div>

              <!--商品列表-->
              <div class="product-box">
                <div>
                  <div class="clearfix pro-title-box">
                    <span class="pro-title">热卖推荐</span>
                </div>
                <div class="product clearfix">
                  <div class="pro-item fl"
                  v-for="(item,index) in hotProduct"
                  :key="index"
                  >
                  <div class="img-box">
                    <img
                        class="auto_img"
                        :src="item.smallImg"
                        alt=""
                        />
                        <!--热卖标签-->
                        <div class="hot">热卖</div>
                  </div>
                  <div class="pro-info">
                    <div class="pro-name one_text">
                     {{ item.name }}
                    </div>
                    <div class="pro-ename one_text">
                      {{ item.enname }}
                    </div>
                    <div class="pro-price">￥{{item.price }}</div>
                    </div>
                  </div>
                </div>
             </div>
           </div>
        </div>
      </div>
</template>

<script>
// @ is an alias to /src
import "../assets/less/home.less";
export default {
  name: "Home",
  data(){
    return{
      //定义一个属性来接收时间
      times:"",
      //接收轮播图数据
      bannerData:[],
      //
      hotProduct:[],

    };
  },
  created(){
    this.getDate();
    this.getBannerData();
    this.getHotProduct();
  },
  methods:{
    //动态获取时间
    getDate(){
      let hour = new Date().getHours();
      //console.log(hour);
      if(hour < 6){
        this.times = "凌晨好~";
      }else if(hour < 11){
        this.times = "早上好~";
      }else if(hour < 13){
        this.times = "中午好~";
      }else if(hour < 18){
        this.times = "下午好~";
      }else if(hour < 23){
        this.times = "晚上好~";
      }
    },
    //获取轮播图数据
    getBannerData(){
      //调用vant中的轻提示里面的loading（加载中）提示
      this.$toast.loading({
        //提示文字
        message:"加载中...",
        //点击穿透
        forbidClick: true,
        //延迟
        duration: 0,

      });
      //发起请求
      this.axios({
        //请求类型
        methor: "GET",
        //请求路径
        url: "/banner",
        //请求参数（object）
        params:{
          appkey: this.appkey,
        },
      })
      .then((result) => {
        this.$toast.clear();
        //console.log(result);
        if(result.data.code == 300){
          this.bannerData = result.data.result;
        }
      })
      .catch((err) => {
        //如果数据获取出现错误，axios也会判断数据加载完毕
        //所以将loding动画清除
        this.$toast.clear();
      });
    },
    getHotProduct(){
      this.$toast.loading({
        message:"加载中...",
        forbidClick:true,
        duration:0,
      });
      //发送请求
      this.axios({
        //请求类型
        method: "GET",
        url: "/typeProducts",
        params:{
          appkey: this.appkey,
          key: "isHot",
          value: 1,
        },
      })
      .then((result) => {
        this.$toast.clear();
        if (result.data.code == 500) {
          this.hotProduct = result.data.result;
        }
      })
      .catch((err) => {
        //如果数据获取错误，axios也会判断数据加载完毕
        //所以将loding动画清除
        this.$toast.clear();
      });
    },
  },
};
</script>
