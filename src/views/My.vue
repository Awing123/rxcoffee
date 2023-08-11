<template>
    <div class="my">
       <div
       class="my-bg"
       v-if="userInfo.userBg"
       :style="{backgroundImage:  `url('${userInfo.userBg}')`}"
       >
       <van-uploader class="upload-box" :after-read="uploading"/>
       </div>
       <div class="my-info">
        <div class="clearfix">
            <div class="my-pic fl">
                <img :src="userInfo.userImg" class="auto_img" alt=""/>
            </div>
            <div class="my-info-box fl">
                <div class="nickname">{{ userInfo.nickName }}</div>
                <div class="user-desc one_text">
                    {{ 
                    userInfo.desc == ""
                    ?"这家伙很懒，什么都没有留下！"
                    :userInfo.desc
                         }}
                </div>
            </div>

            <!--单元格跳转-->
            <div class="list-box">
                <van-cell
                v-for="(item,index) in listData"
                :key="index"
                :title="item.title"
                is-link
                @click="toPage(item.name)"
                />
            </div>
        </div>
       </div>
    </div>
</template>

<script>
export default{
    name:"My",
    data(){
        return{
            userInfo:{},
            listData:[
                {
                    title:"个人资料",
                    name:"Account",
                },
                {
                    title:"我的订单",
                    name:"Order",
                },
                {
                    title:"我的收藏",
                    name:"like",

                },
                {
                    title:"地址管理",
                    name:"Address",

                },
                {
                    title:"浏览足迹",
                    name:"Track",

                },
                {
                    title:"安全中心",
                    name:"Secure",

                },
            ],
        };
    },
    created(){
        //获取用户信息
        this.getUserInfo();
    },
    methods:{
        toPage(name){
            this.$router.push({name});
        },
        uploadBg(file){
            //允许上传的文件类型
            let type = ['jpg','png','gif','jpeg','web'];
             //允许最大大小1mb
             let size = 1;

             //判断文件类型
             let fileType= file.file.type.split('/')[1];
             //查看读取文件的类型
             if(type.indexOf(fileType)=== -1){
                this.$toast(`文件类型仅支持${type.join(',')}`);
                return;
             }
             let fileSize=file.file.size / 1024 /1024;

             if(fileSize > size){
                this.$toast(`文件最大不能超过${size}MB`);
                return;
             }
             let base64 = file.content.replace(/^data:image\/[A-Za-z]+;base64,/,'');

            //发送请求
            let tokenString = localStorage.getItem("token");

            if(!tokenString) {
                this.$toast("请去登录~");
                return this.$router.push({name:"Login"});
            }

            this.$toast.loading({
                message:"获取信息中...",
                forbidClick: true,
                duration: 0,
            });

            this.axios({
                method: "POST",
                url: "/updateUserBg",
                data: {
                        appkey: this.appkey,
                        toKenString,
                        imgType:fileType,
                        serverBase64Img:base64
                    },
        })

            .then((resulet) => {
                this.$toast.clear();
                if(resulet.data.code == 700) {
                    this.$router.push({name:"Login"});
                }else if(resulet.data.code =="I001"){
                    this.userInfo.userBg=result.data.userBg;
                }
                this.$toast(result.data.msg);
            })
            .catch((err) => {
                this.$toast.clear();
            });
        },
        getUserInfo(){
            let tokenString = localStorage.getItem("token");

            if(!tokenString) {
                this.$toast("请去登录~");
                return this.$router.push({name:"Login"});
            }

            this.$toast.loading({
                message:"获取信息中...",
                forbidClick:true,
                duration:0,
            });

            this.axios({
                method:"GET",
                url:"/findMy",
                params:{
                    appkey:this.appkey,
                    tokenString,
                },
            })
            .then((result) => {
                this.$toast.clear();
                if(result.data.code == 700){
                    this.$router.push({name:"Login"});
                }else if(result.data.code == "A001"){
                    this.userInfo = result.data.result[0];
                }
            })
            .catch((err) => {
                this.$toast.clear();
            });
        },

    },
};
</script>