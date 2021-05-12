<template>
  <div class="background">
      <div class="mainBox">
          <div class="topBox"></div>
          <div class="centerBox">
              <div class="leftBox"><left-menu></div>
              <div class="rightBox">
                  <transition :nema="transitionName"></transition>
                  <router-view></router-view>
              </div>
          </div>
          <div class="footerBox"></div>
      </div>
      <div class="left_Box"></div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      transitionName: "",
    };
  },
   watch: {
    // 监听路由跳转
    //to: Route，代表要进入的目标，它是一个路由对象
    //from: Route，代表当前正要离开的路由，同样也是一个路由对象
    $route(to, from) {
      if (from.meta.index == 0 && to.meta.index > from.meta.index) {
        //设置动画名称
        this.transitionName = "slide-left";
        return;
      }
      if (to.meta.index == 0 && to.meta.index < from.meta.index) {
        this.transitionName = "slide-right";
      }
      if (to.meta.index > 1 && to.meta.index > from.meta.index) {
        //设置动画名称
        this.transitionName = "slide-com";
      }
      if (to.meta.index > 1 && to.meta.index < from.meta.index) {
        //设置动画名称
        this.transitionName = "slide-back";
      }
    },
  },
};
</script>

<style>
</style>