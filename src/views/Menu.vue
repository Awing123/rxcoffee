<template>
    <div class="menu">
        <div class="menu-vav">
            <div class="menu-search">
                <van-search shape="round" placeholder="输入商品名称" />
            </div>
            <div class="menu-option">
                <div class="menu-item" 
                v-for="(item, index) in menuOption" 
                :key="index"
                @click="toMneu(index,item.type)"
                >
                    <div class="m-item" :class="{ active: menuIndex ==index}">
                        <div class="m-icon">
                            <img 
                            class="auto_img" 
                            :src="
                                menuIndex == index
                                    ? item.activeIcon
                                    : item.inactiveIcon
                            " 
                            alt="" 
                            />
                        </div>
                        <div class="m-text">
                            {{ item.title }}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    <div class="menu-product">
        <div
        class="clearfix m-pro-item"
        v-for="(item,index) in productData"
        :key="index"
        >
        <div class="fl pro-img">
            <img class="auto_img" :src="item.smallImg" alt=""/>
        </div>
        <div class="fl text">
            <div class="pro-text">
                <div class="pro-name one_text">{{ item.name }}</div>
                <div class="pro-ename one_text">{{ item.enname }}</div>
            </div>
        </div>
              <div class="fl price">￥{{ item.price }}</div>
            </div>
       </div>
    </div>
</template>

<script>
import "../assets/less/menu.less"
export default {
    name: "Menu",
    data() {
        return {
            menuIndex: 0,
            menuOption: [
                {
                    title: "热卖",
                    // 选中的下标
                    activeIcon: require("../assets/images/icons_18.gif"),
                    // 未选中的下标
                    inactiveIcon: require("../assets/images/icons_03.gif"),
                },
                {
                    title: "拿铁",
                    // 选中的下标
                    activeIcon: require("../assets/images/icons_19.gif"),
                    // 未选中的下标
                    inactiveIcon: require("../assets/images/icons_05.gif"),
                },
                {
                    title: "咖啡",
                    // 选中的下标
                    activeIcon: require("../assets/images/icons_03.gif"),
                    // 未选中的下标
                    inactiveIcon: require("../assets/images/icons_05.gif"),
                },
                {
                    title: "瑞纳冰",
                    // 选中的下标
                    activeIcon: require("../assets/images/icons_03.gif"),
                    // 未选中的下标
                    inactiveIcon: require("../assets/images/icons_05.gif"),
                },
                {
                    title: "水果茶",
                    // 选中的下标
                    activeIcon: require("../assets/images/icons_03.gif"),
                    // 未选中的下标
                    inactiveIcon: require("../assets/images/icons_05.gif"),
                },
            ],
            //读取数据
            productData:[],
        };
    },
    //?
    created(){
        //获取显示各类型的数据
        this.getType();
        //获取热卖界面
        this.getProductByType("isHot",1);
    },
    methods: {
        //获取商品类型
        toMneu(index, type) {
            if (this.menuIndex == index) {
                return;
            }
            this.menuIndex =index;
            this.getProductByType(type);
        },
        //获取商品类型
        getType(){
            this.$toast.loading({
                message: "加载中...",
                forbideClick: true,
                duration: 0,
            });
            //发送请求
            this.axios({
                //请求类型
            mehod:"GET",
            url:"/type",
            params: {
                appkey: this.appkey,
            },
        })
        .then((result) => {
            this.$toast.clear();
            if(result.data.code == 400) {
                let data = result.data.result;
                //插入数据类型到data里面
                data.unshift({
                    id:5,
                    type: "isHot",
                    typeDesc: "热卖推荐",
                    createdAt: "2022-12-24T09:57:57.000Z",
                    updatedAt: "2021-01-24T09:57:57.000Z",
                });
                //map()用于返回新数据
                this.menuOption.map((v) => {
                    for(let i = 0;i < data.length; i++){
                        //匹配title和typeDesc
                        if(v.title == data[i].typeDesc) {
                            v.type = data[i].type;
                            break;
                        }
                    }
                });
                let type = this.menuOption[this.menuIndex].type;
                //调用根据商品类型或企业商品的方法
                this.getProductByType(type);
            }
        })
        .catch((err) =>{
            this.$toast.clear();
        });
    },
    getProductByType(type) {
        let params = {
            appkey: this.appkey,
        };
        if(type == "isHot"){
            params.key = "isHot";
            params.value = 1;
        } else{
            params.key="type";
            params.value=type;
        }
        this.$toast.loading({
                message: "加载中...",
                forbideClick: true,
                duration: 0,
            });
            //发送请求
            this.axios({
                //请求类型
            mehod:"GET",
            url:"/typeProducts",
            params,
            })
            .then((result) => {
                this.$toast.clear();
                if(result.data.code == 500){
                    this.productData = result.data.result;
                }
            })
            .catch((err) =>{
                this.$toast.clear();
            });
        },
    },
};
 

</script>