<template>
  <div class="div">
    <div class="div1">
      <div class="div2">
        <img class="img img1" src="../../assets/img/shouji.png" alt>
        <input class="text1" type="text" placeholder="请输入手机号" v-model="phone">
      </div>
      <div class="div2">
        <img class="img" src="../../assets/img/erweima.png" alt>
        <input class="text2 text3" type="text" placeholder="请输入验证码" v-model="code">
        <p class="span" v-if="inentify" @click="send" style="background:#12377d ; color:#fff">获取验证码</p>
        <p class="span" v-if="timer">{{ count }}s</p>
      </div>
      <div class="div2">
        <img class="img" src="../../assets/img/suo.png" alt>
        <input class="text2" type="password" placeholder="请重置密码" v-model="password">
      </div>
      <div class="div2">
        <img class="img" src="../../assets/img/suo.png" alt>
        <input class="text2" type="password" placeholder="请确认密码" v-model="password1">
      </div>
      <button @click="top()">确定</button>
    </div>
  </div>
</template>
<script>
// import axios from "axios";
// import func from "./vue-temp/vue-editor-bridge";
export default {
  data() {
    return {
      msg: "找回密码",
      msg1: "获取验证码",
      bool: false,
      inentify: true,
      timer: false,
      count: 60,
      phone: "",
      code: "",
      password: "",
      password1: ""
    };
  },
  methods: {
    //   最后用户确定的事件
    async top() {
      if (!this.phone || !this.password || !this.password1 || !this.code) {
        this.$toasted.error("请输入完善信息", { icon: "error" }).goAway(2000);
        return;
      }

      try {
        // await等待一个异步返回的结果 如果没有await 会报user is undefined 获取不到
        let res = await this.http.post("/api/forget_make ", {
          phone: this.phone,
          password: this.password,
          password_confirmation: this.password1,
          sms_code: this.code
        });
        console.log(res);
        this.$toasted.success("修改成功").goAway(1500);
        this.$router.replace({ name: "login" });
      } catch (error) {
        this.$toasted.error(error.message, { icon: "error" }).goAway(2000);
      }
    },
    // 短信验证的事件
    async send() {
      if (!this.phone) {
        this.$toasted.error("请输入手机号", { icon: "error" }).goAway(2000);
        return;
      }
      try {
        // await等待一个异步返回的结果 如果没有await 会报user is undefined 获取不到
        let res = await this.http.post("api/send_code", {
          phone: this.phone
        });
        console.log(res);
        this.$toasted.success("发送成功").goAway(1500);
      } catch (error) {
        this.$toasted.error(error.message, { icon: "error" }).goAway(2000);
      }
      (this.inentify = !this.inentify),
        (this.timer = true),
        (this.time = setInterval(() => {
          this.count -= 1;
          if (this.count == 0) {
            clearInterval(this.time);
            this.inentify = !this.inentify;
            this.timer = false;
            this.count = 60;
          }
        }, 1000));
    }
  },
  mounted() {
    this.$store.commit("size", "找回密码");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    this.$store.commit("fanhui", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("size", this.$route.params.name);
    this.$store.commit("fanhui", true);
  }
};
</script>
<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../../assets/font/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}

.div {
  padding-top: 0.88rem;
  min-height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.div1 {
  width: 92%;
  height: 10rem;
  margin: 0 auto;
}
input {
  height: 0.9rem;
  width: 95%;
  margin: 0 auto;
  padding: 0 0.9rem;
}
.text1 {
  margin-top: 0.3rem;
  margin-bottom: 0.1rem;
}
.text2 {
  margin-bottom: 0.1rem;
}
.text3 {
  width: 66%;
}
.span {
  display: inline-block;
  height: 0.9rem;
  background: #ffffff;
  text-align: center;
  line-height: 1rem;
  margin-left: 0.16rem;
  width: 27%;
}
button {
  margin-top: 0.58rem;
  width: 94%;
  height: 0.88rem;
  background: #c0a580;
  border-radius: 0.5rem;
}
.div2 {
  position: relative;
}
.img {
  height: 0.4rem;
  position: absolute;
  top: 0.26rem;
  left: 0.4rem;
}
.img1 {
  top: 0.56rem;
}
</style>