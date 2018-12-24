<template>
  <div class="div">
    <div>
      <span>旧密码</span>
      <input type="password" v-model="oldpassword" placeholder="请输入旧密码">
    </div>
    <div>
      <span>新密码</span>
      <input type="password" v-model="password" placeholder="请输入新密码">
    </div>
    <div>
      <span>确定新密码</span>
      <input type="password" v-model="password1" placeholder="请再次输入新密码">
    </div>
    <button @click="pay()">确定修改</button>
  </div>
</template>
<script>
import { Toast } from "mint-ui";
import Axios from "axios";
export default {
  data() {
    return {
      password: "",
      password1: "",
      oldpassword: ""
    };
  },
  mounted() {
    this.$store.commit("size", "设置支付密码");
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
  },
  methods: {
    pay() {
      var _this = this;
      _this.http
        .post("/api/center/password_make", {
          old_password: _this.oldpassword,
          password: _this.password,
          password_confirmation: _this.password1,
          type: "2"
        })
        .then(res => {
          _this.$router.push("/");
          _this.$toasted.success("修改成功").goAway(1500);
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
        });
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.div > div {
  margin-left: 0.4rem;
  margin-right: 0.4rem;
  font-size: 0.3rem;
  color: #fff;
  border-bottom: 1px solid #5b91eb;
}
.div > div > span {
  display: inline-block;
  padding-top: 0.66rem;
  padding-bottom: 0.38rem;
  padding-left: 0.52rem;
  width: 2.16rem;
}
button {
  height: 0.8rem;
  width: 90%;
  margin-left: 5%;
  margin-top: 0.84rem;
  color: #fff;
  font-size: 0.3rem;
  border-radius: 4px;
  background: #de9f27;
}
.div > div > input {
  background: none;
  background-size: cover;
  color: #fff;
  font-size: 0.3rem;
}
</style>
