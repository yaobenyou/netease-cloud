<template>
    <div class="box">
        <div class="left">
            <img src="" alt="">
        </div>
        <div class="right">
            <div class="boxLeft">
                <div><i class="el-icon-arrow-left"></i></div>
                <div><i class="el-icon-arrow-right"></i></div>
            </div>
            <div class="boxCenter h">
                <input type="text" @keyup.enter = "clickSearch()" v-model="inputContent" clearable>
                <i class="el-icon-search" @click="clickSearch()"></i>
            </div>

            <div class="topRight">
                <div class="boxRight"></div>
                <div class="boxRight"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "topHead",
//   数据模型
  data() {
    return {
      serchContent: "",
      inputContent: "",
      logOutShow: true,
      profile: {
        accountStatus: 0,
        authStatus: 0,
        authority: 0,
        avatarImgId: 109951164677081620,
        avatarImgIdStr: "109951164677081622",
        avatarImgId_str: "109951164677081622",
        avatarUrl:
          "https://p4.music.126.net/CTdiLpyZnnaQvoaS6Qheow==/109951164677081622.jpg",
        backgroundImgId: 109951164821696270,
        backgroundImgIdStr: "109951164821696277",
        backgroundUrl:
          "https://p3.music.126.net/BbqNnT8No61xX901-40EbA==/109951164821696277.jpg",
        birthday: -2209017600000,
        city: 130600,
        defaultAvatar: false,
        description: "",
        detailDescription: "",
        djStatus: 0,
        eventCount: 0,
        expertTags: null,
        experts: {},
        followed: false,
        followeds: 2,
        follows: 10,
        gender: 1,
        mutual: false,
        nickname: "雨西",
        playlistBeSubscribedCount: 0,
        playlistCount: 4,
        province: 130000,
        remarkName: null,
        signature: "",
        userId: 547016550,
        userType: 0,
        vipType: 0,
      },
    };
  },
  created() {
      this.showProfile();
  },
    methods: {
        // 退出登陆
        clickLogout(){
            // 退出操作 --> 清空token
            window.sessionStorage.clear();
            this.$router.push("/login");
            alert("账号已退出 !");
        },
        // 显示个人信息
        showProfile(){
            // 从内存中获取
            this.profile = window.localStorage.getItem("getP");
        },
        // 后退
        clickLeft(){
            if (this.$route.path === "/home/find/geXing") {
                return;
            }
            this.$router.go(-1);
        },
        // 前进
        clickRight(){
            this.$router.go(1);
        },
        // 搜索点击
        clickSearch(){
            this.$router.push("/home/search"+this.inputContent).catch(err => console.log(err));
        }
    }   
};
</script>

<style>
</style>