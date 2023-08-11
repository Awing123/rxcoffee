<template>
  <div class="main">
<!--二级路由-->
<div>
<router-view></router-view>
</div>

<!--底部导航栏-->
<van-tabbar
            v-model="activeName"
            active-color="#ee0a24"
            inactive-color="#000"
            route
            >
  <van-tabbar-item 
  v-for="(item, index) in tabbarData"
  :key="index"
  :icon="item.icon"
  replace
  :to="item.path"
  :name="item.name"
  >
  <span>{{ item.title }}</span>
  <template #icon="props">
<img
    :src="props.active ? item.activeImg : item.inactiveImg"
    />
</template>
</van-tabbar-item>
</van-tabbar>
</div>
</template>
<script>
export default{
          name:"Main",
          data(){
            return{
              //激活标签
                activeName: "Home",
                tabbarData: [
                {
                    title: "首页",
                    icon: "wap-home-o",

                    // 标签名称、页面跳转路由名称
                    name: "Home",
                    path: "/main/home",

                    // 未选中和选中图标的路径
                    inactiveImg: require("../assets/images/home.png"),
                    activeImg: require("../assets/images/home_active.png"),
                },
                {
                    title: "菜单",
                    icon: "wap-home-o",

                    // 标签名称、页面跳转路由名称
                    name: "Menu",
                    path: "/main/menu",

                    // 未选中和选中图标的路径
                    inactiveImg: require("../assets/images/menu.png"),
                    activeImg: require("../assets/images/menu_active.png"),
                },
                {
                    title: "购物袋",
                    icon: "wap-home-o",

                    // 标签名称、页面跳转路由名称
                    name: "Shopbag",
                    path: "/main/shopbag",

                    // 未选中和选中图标的路径
                    inactiveImg: require("../assets/images/shopbag.png"),
                    activeImg: require("../assets/images/shopbag_active.png"),
                },
                {
                    title: "我的",
                    icon: "wap-home-o",

                    // 标签名称、页面跳转路由名称
                    name: "My",
                    path: "/main/my",

                    // 未选中和选中图标的路径
                   inactiveImg: require("../assets/images/my.png"),
                    activeImg: require("../assets/images/my_active.png"),
                },
            ],
         };

     },
};

</script>