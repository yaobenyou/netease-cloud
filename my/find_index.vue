<template>
  <div class="maxBox">
    <div class="topBar" ref="right_Box_Ref" id="right_box">
      <tap-bar></tap-bar>
    </div>
    <transition :name="transitionName">
      <router-viem></router-viem>
    </transition>
  </div>
</template>

<script>
import tapBar from "components/find?tapBar";
export default {
  data() {
    return {
      transitionName: "",
    };
  },
  components: {
    tapBar,
  },
  watch: {
    $route(to, from) {
      if (to.meta.index > form.meta.index) {
          this.transitionName = "slide-left";
      }
      if (to.meta.index < form.meta.index) {
          this.transitionName = "slide-right"
          
      }
    },
  },
};
</script>

<style>
</style>