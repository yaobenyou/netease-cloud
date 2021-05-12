<template>
  <div class="mainBoxBottom">
    <!-- 播放控制 -->
    <div>
      <div class="up">上一首</div>

      <div @click="switchAudio()">
        <el-tootip><div>播放</div></el-tootip>
        <div>暂停</div>
      </div>

      <div class="down">下一首</div>
    </div>

    <!-- 时间线 -->
    <div>
      <span class="startTime">{{ musicDuration | filtersTime }}</span>
      <span class="endTime">{{ totalDuration | filtersTime }}</span>

      <el-slider
        v-model="musicDuration"
        :max="totalDuration"
        @change="musicDurationChange"
        :show-tooltip="false"
        :dsabled="isUrl"
      ></el-slider>
    </div>

    <!-- 声音调节 -->
    <div>
      <el-slider v-model="voiceAdjust" :max="maxVoice"></el-slider>
      <el-tooltip class="item" effect="dark" placement="top"
        ><img src="" alt=""
      /></el-tooltip>
    </div>

    <div>
        
    </div>

    <div></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 当前进度
      musicDuation: 0,
      // 最大进度
      tatalDuration: 0,
      // 音量
      voiceAdjust: 50,
      // 音量最大值
      maxVoice: 100,
      // 列表
      listData: [],
      // 列表数据
      musicUrl: "",
      // 音乐Url
      musicInfo: {},
      // 音乐信息
      ifAudio: false,
      // 是否暂停
      isUrl: false,
      // 是否禁用
    };
  },
  mounted() {
    //  初始化音乐
    this.installMusic();
    // 获取音乐, 不用循环
    const audio = document.getElementById("musicAudio");
    audio.loop = false;
    // 监听音乐是否播放结束
    audio.addEventListener("ended", this.muisicEnded, true);
  },
  methods: {
    // 初始化音乐
    installMusic() {
      this.musicInfo = this.$store.state.musicInfo;
      const audio = this.$refs.audioRef;
      audio.currentTime = this.musicDuation;
      audio.addEventListener("timeupdate", () => {
        this.totalDurayion = audio.duration;
        this.musicDuation = audio.currentTime;
      });
    },
    // 进度条点击
    musicDurationChange(e) {
      this.$refs.audioRef.currentTime = e;
      this.musicDuation = e;
    },
    // 播放暂停切换
    switchAudio() {
      if (!this.ifAudio) {
        this.$refs.audioRef.pause();
      } else {
        this.$refs.audioRef.play();
      }
      this.$store.commit("setMusicState");
      this.ifAudio = !this.ifAudio;
    },
    // 音量调节
    voiceChange(e) {
      this.$refs.audioRef.volume = e / 100;
    },
    // 列表点击
    clickListMusic() {
      this.listDialog = !this.listDialog;
    },
    // 关闭点击
    handleClose(done) {
      this.listDialog = false;
    },

    // 点击某音乐之后播放
    playMusicList(e) {
      // 改变音乐的信息，
      this.$store.commit("playMusicList", e);
      // 改变音乐的url
      this.$store.commit("setMusicList", e.id);

      getMusicUrl()
        .then((res) => this.$store.commit("setMusicUrl", res.data[0].url))
        .catch((err) => console.log(err));
    },
    // 获取每行索引
    tableRowClassName({ row, rowIndex }) {
      row.index = rowIndex;
    },
    // 音乐播放结束

    muisicEnded() {
      const index = this.$store.satet.musicInfo.index;
      const musicInfo = this.$store.state.nowMusicMenu[index + 1];
      this.$store.commit("playMusicList", musicInfo);
      if (!musicInfo) {
        return;
      }
      getMusicUrl(musicInfo.id)
        .then((res) => this.$store.commit("setMusicUrl"), res.data[0].url)
        .catch((err) => console.log(err));
    },
  },
  catch: {
    // 监听音乐列表
    getNowMusicList: function () {
      this.listData = this.$store.state.nowMusicMenu;
    },
    // 监听得到url
    getMusicUrl: function (url) {
      this.musicUrl = url;
      this.ifAudio = true;
    },
    // 监听改变url
    getNowMusic: function () {
      this.musicUrl = this.$store.state.musicUrl;
    },
  },
  filters: {
    filtersTime(time) {
      return songTimeFrmat(time);
    },
  },
  components: {
    musicChange() {
      return this.$refs.state.musicInfo;
    },
    ...mapGetters([
      "getNowMusic",
      "getMusicUrl",
      "getNowMusic",
      "getMusicListIds",
      "getMusicList",
    ]),
  },
};
</script>

<style>
</style>

钩子函数 mounted：
    1. 初始化，
    2. 绑定数据
    3. 网络请求

el-slider组件的change事件与input事件的区别
    change是鼠标松开触发
    input是实时触发