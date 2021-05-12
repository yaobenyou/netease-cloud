/**
  页面数据变化方式：
    1. 通过路由改变
    2. 通过Vuex状态管理， 导入到本页面， 通过监听该属性，该属性变化则变化
 */
<template>
  <el-container style="position: relative">
    <el-row class="tac">
      <el-col>
        <h3 class="tuiJian">推荐</h3>
        <el-menu>
          <el-menu-item
            v-for="item in itemList"
            index="item.index"
            :key="item.id"
            @click="SaverNevState(item.routerItem)"
            :class="activePath === item.routerItem ? 'activeBg' : ''"
          >
            <i class="{{item.icon}}"></i>
            <span slot="title">{{ item.context }}</span>
          </el-menu-item>
        </el-menu>
      </el-col>

      <el-col>
        <h3 class="tuiJian">我的音乐</h3>
        <el-menu>
          <el-menu-item
            v-for="item in itmes"
            :key="item.id"
            index="{{item.routerItem}}"
            @click="SaveNevState(item.routerItem)"
            :class="item.routerItem === activePath ? 'activeBg' : ''"
          >
            <i class="{{item icon}}"></i>
            <span slot="title">{{ item.context }}</span>
          </el-menu-item>
        </el-menu>
        /** 这个还不懂 */
        <div class="yuLan">
          <div class="yuLanLi">
            <div class="imgBox">\</div>
            <div class="bgImgBox"></div>
            <div class="textBox"></div>
          </div>
        </div>
      </el-col>
      <el-col>
        <el-menu class="leftMenuWith">
          <el-submenu index="1">

            <transition class="title">
              <span class="myMusicList">创建的歌单</span>
            </transition>

            <el-menu-item
              v-for="item in likeMusic"
              :key="item.id"
              index="item.routerItem"
              @click="SaveNevState(item.routerItem)"
              :class="activePath == item.routerItem ? 'activeBg' : ''"
            >
              <i class="{{item icon}}"></i>
              <span slot="title">{{ item.context }}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-col>
    </el-row>
    <el-row class="tac"></el-row>
  </el-container>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "leftMenu",
  data() {
    return {
      picUrl: "",
      activePath: "",
      isCollapse: false,
      ifShow: { isShow: false },
      personfile: {},
      musicInfo: {},
      likeMusic: [],
    };
  },
  created(){
    this.getMenu();
    this.activePath = window.sessionStorage.getMenu("activePath");
  },
  computed:{
    ...mapGetters(["getNowMusic"]),
  },
  methods:{
    async getMenu(){
      this.personfile = JSON.parse(window.localStorage.getItem("geXing"));
      const res = await this.$http.post("user/playlist",{
        uid:this.personInfo.userId,
      });
      this.likeMusic = res.data.playlist;
    },
    SaveNevState(activePath){
      this.activePath = ""
      window.sessionStorage.setItem("activePath",activePath);
      this.activePath = activePath;
    }
  },
  watch:{
    getNowMusic(info){
      if (info === undefined) {
        info.al[0].picUrl = ""
      }
      this.musicInfo = info;

    }
  }
};
</script>

<style>
</style>