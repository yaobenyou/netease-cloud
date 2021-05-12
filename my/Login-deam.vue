<template>
  <div class="dg">
    <el-form
      class="login_form"
      ref="right-top"
      :rules="loginFormRules"
      :model="loginForm"
      label-width="0px"
    >
      <el-form-item prop="username">
        <el-input v-model="loginForm.phone" placeholder="请输入账号"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          v-model="loginForm.password"
          placeholder="请输入密码"
        ></el-input>
      </el-form-item>

      <el-form-item class="btns">
          <el-button type="primaryt" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
    
<script>
export default {
  name: "Login",
  // 登录的数据模型

  data() {
    return {
      loginForm: {
        phone: "14770025460",
        password: "yuxi1314",
      },
      loginFormRules: {
        phone: {
          required: true,
          message: "请输入手机号",
          trigger: "blur",
        },

        password: {
          required: true,
          massage: "请输入登录密码",
          trigger: "blur",
        },
      },
    };
  },
  // 登录的规则
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    //   成功则保存在本地
    async login() {
      const res = await this.$http.post("login/cellphone", this.lohinFrom);

      if (res.statusText === "OK") {
        window.localStorage.setItem("getP", JSON.stringify(res.data.profile));
        this.$store.commit("getLogin", true);
        return this.$router.push("/home/find/geXing");
      } else if (res.statusText !== "OK") {
        this.loginShow = false;
        return this.$message.error(
          "登录失败，你输入的手机号或者密码有误，请重新输入！"
        );
      } else {
        return console.log(res);
      }
    },
  },
};
</script>

<style>
</style>