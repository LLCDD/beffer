<template>
  <div class="content">
    <div class="op"></div>
    <div class="divll">
      <i class="iconfont img1 img2">&#xe628;</i>
      <input class="phone" type="text" placeholder="用户名" v-model="phone">
    </div>
    <div class="divl">
      <i class="iconfont img1">&#xe613;</i>
      <input type="password" placeholder="用户密码" v-model="password">
    </div>
    <div class="login" @click="login">登录</div>
    <div class="problem">
      <router-link tag="span" to="/register/no">注册会员</router-link>
      <router-link tag="span" to="/retrieve/忘记密码">找回密码</router-link>
    </div>
  </div>
</template>

<script>
import router from "@/router";
export default {
  data() {
    return {
      phone: "",
      password: ""
    };
  },

  methods: {
    async login() {
      var _this = this;
      if (!this.phone || !this.password) {
        this.$toasted.error("请输入完善信息", { icon: "error" }).goAway(2000);
        return;
      }

      try {
        // await等待一个异步返回的结果 如果没有await 会报user is undefined 获取不到
        let res = await this.http.post("/api/login_make", {
          phone: this.phone,
          password: this.password
        });
        if (res.code == 200) {
          console.log(res);
          this.$toasted.success("登录成功").goAway(1500);
          this.$router.push("/index");
          localStorage.setItem("username", this.phone);
          localStorage.setItem("token", res.data.token);
          localStorage.setItem("img", JSON.stringify(res.data.banners));
          if (localStorage.getItem("username")) {
            this.$store.commit("tabBar", true);
            this.$store.commit("tishi", true);
            this.$store.commit("headerStatus", true);
          } else {
            this.$store.commit("tabBar", false);
            this.$store.commit("tishi", false);
            this.$store.commit("headerStatus", false);
          }
        } else {
          this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          this.$store.commit("tabBar", false);
          this.$store.commit("tishi", false);
          this.$store.commit("headerStatus", false);
        }
        //
      } catch (error) {
        this.$toasted.error(error.message, { icon: "error" }).goAway(2000);
      }
    }
  },
  mounted() {
    this.$store.commit("tabBar", false);
    if (this.$route.query.phone) {
      localStorage.setItem("phone", this.$route.query.phone);
    }
  }
};
</script>

<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../../../assets/font1/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}

.content {
  width: 80%;
  margin: 0 auto;
  border-radius: 0.1rem;
  margin-top: 4.6rem;
  z-index: 1;
  position: relative;
  min-height: 6rem;
}
.op {
  width: 100%;
  height: 4.5rem;
  margin: 0 auto;
  margin-top: 0.3rem;
  position: absolute;
  background: #e0e0e0;
  opacity: 0.2;
  border-radius: 0.1rem;
}
.phone {
  margin-top: 0.72rem;
  margin-bottom: 0.1rem;
}
.content > :nth-child(2) {
  border-radius: 0;
}
.code {
  border-bottom-left-radius: 0.15rem;
  border-bottom-right-radius: 0.15rem;
  width: 60%;
}
.content input {
  width: 80%;
  height: 0.8rem;
  margin-left: 0.6rem;
  border: 0;
  color: #fff;
  border-bottom: 1px solid #fff;
  background: none;
  padding: 0.05rem 0rem 0.05rem 0.8rem;
}
.login {
  width: 80%;
  height: 0.8rem;
  border-radius: 0.1rem;
  background: #d79d12;
  margin-left: 0.6rem;
  margin-top: 0.5rem;
  text-align: center;
  line-height: 0.8rem;
  font-size: 0.3rem;
  position: relative;
  color: #fff;
  z-index: 11;
}
.problem {
  width: 80%;
  /* height: 0.33rem; */
  display: flex;
  position: relative;
  justify-content: space-between;
  margin-left: 0.6rem;
}
.problem span {
  width: 1.2rem;
  /* height: 0.44rem; */
  /* line-height: 0.44rem; */
  text-align: center;
  font-size: 0.24rem;
  color: #d79d12;
  margin-top: 0.3rem;
}
.divl {
  position: relative;
  width: 100%;
  margin: 0 auto;
}
.img1 {
  width: 0.4rem;
  display: inline-block;
  position: absolute;
  top: 0.25rem;
  left: 0.8rem;
  color: #fff;
}
.divl1 {
  height: 80%;
  width: 26%;
  position: absolute;
  top: 10%;
  right: 2%;
  font-size: 0.5rem;
  padding: 1% 4%;
}
.divll {
  position: relative;
}
.img2 {
  position: absolute;
  top: 0.95rem;
  left: 0.8rem;
  color: #fff;
}
.div2 {
  margin-top: 0.1rem;
}
</style>