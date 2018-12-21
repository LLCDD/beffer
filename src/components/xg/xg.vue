<template>
  <div class="div">
    <div>
      <p>旧密码</p>
      <input type="password" placeholder="请输入旧密码" v-model="password1">
    </div>
    <div>
      <p>新密码</p>
      <input type="password" placeholder="请输入新密码" v-model="password2">
    </div>
    <div>
      <p>确定新密码</p>
      <input type="password" placeholder="请确认新密码" v-model="password3">
    </div>
    <button @click="xg()">确定修改</button>
  </div>
</template>
<script>
import axios from "axios";
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      msg: "修改密码",
      password1: "",
      password2: "",
      password3: ""
    };
  },
  mounted() {
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("size", "设置密码");
    this.$store.commit("fanhui", true);
  },
  methods: {
    xg() {
      const _this = this;
      _this.http
        .post("/api/center/password_make", {
          old_password: _this.password1,
          password: _this.password2,
          password_confirmation: _this.password3,
          type: "1"
        })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success("修改成功,请从新登陆").goAway(1000);
            _this.$router.push("/");
          } else if (res.code == 400) {
            this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  height: 100%;
}
.div > div > p {
  display: inline-block;
  font-size: 0.3rem;
  color: #fff;
  width: 1.8rem;
}
.div > div {
  margin-left: 0.4rem;
  margin-right: 0.4rem;
  padding-left: 0.54rem;
  padding-top: 0.6rem;
  padding-bottom: 0.38rem;
  border-bottom: 1px solid #5b91eb;
}
.div > div > input {
  display: inline-block;
  color: #fff;
  background: none;
  width: 60%;
}
button {
  width: 90%;
  height: 0.8rem;
  background: #de9f27;
  margin-left: 5%;
  color: #ffffff;
  margin-top: 0.9rem;
  border-radius: 4px;
}
</style>