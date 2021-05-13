<template>
  <body>
    <div class="bthBox">
      <button @click="start">开始</button>
      <button @click="end">结束</button>
      <button @click="hidden">关闭</button>
      <button @click="showClick">显示</button>
    </div>
    <div class="show">
      <listitem></listitem>
    </div>
  </body>
</template>

<script>
import { scrollAuto } from "common/tool";
import ListItem from "./listItem";
export default {
  data() {
    return {
      info: [],
      setTime: null,
      show: true,
    };
  },
  components: {
    listitem,
  },
  created() {
      this.getCommentList();
  },
  mounted(){
      this.start();
  },
  methods: {
    // 返回信息列表
    async getCommentList() {
      const res = await this.$http.get("comment/hotwall/list");
      this.info = res.data.data;
    },
    // 开始滚动
    start() {
      this.time = setInterval(() => {
        let listItem = document.getElementById("leftId");
        let a = listItem.scrollHeight;
        let b = listItem.clientHeight;
        let c = listItem.scrollTop;
        // 滚动条长度 / 当前高度 /  滚动条上面长度

        if (a - b === c) {
          listItem.scroll(a, 0);
        }
        listItem.scrollBy(0, 1);
      }, 50);
    },
    // 结束滚动
    end() {
        clearInterval(this.setTime);
    },
    // 隐藏
    hidden(){
        document.getElementById("left-Box").style.display = "none";
    },
    // 显示
    showClick(){
        this.show = true;
    }
  },
};
</script>

<style>
</style>