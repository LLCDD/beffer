<template>
  <div class="div">
    <div class="div1" :class="{div3:bool}">
      <span>对方账户</span>
      <input type="text" placeholder="对方手机号/钱包地址/ID" v-model="num">
    </div>
    <button @click="top()">下一步</button>
    <div class="div2" v-if="bool">
      <div style="margin-top:0.3rem">
        <span>对方账号：</span>
        <input type="text" v-model="num" placeholder="请输入转账账号" value="num">
      </div>
      <div>
        <span>转账金额：</span>
        <input type="text" v-model="money" placeholder="请输入转入金额">
      </div>
      <div>
        <span>支付密码：</span>
        <input type="password" v-model="password" placeholder="请输入支付密码 ">
      </div>
      <button class="btn" @click="over()">取消转账</button>
      <button class="btn btn1" @click="ok()">确认转账</button>
    </div>
    <div class="op" v-if="op"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "交易",
      num: "",
      bool: false,
      password: "",
      money: "",
      op: false
    };
  },
  methods: {
    top() {
      if (this.num == "") {
        this.$toasted.error("账号不能为空", { icon: "error" }).goAway(2000);
      } else {
        this.bool = true;
        this.op = true;
      }
    },
    over() {
      this.bool = false;
      this.op = false;
      this.num = "";
    },
    ok() {
      const _this = this;
      _this.op = false;
      _this.bool = false;
      _this.http
        .post("/api/asset/transfer_make", {
          account: _this.money,
          security: _this.password,
          phone: _this.num
        })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success(res.message).goAway(1200);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
        });
    }
  },
  mounted() {
    // if (localStorage.getItem("phone")) {
    //   this.num = localStorage.getItem("phone");
    // }
    this.$store.commit("tishi1", false);
    this.$store.commit("size", "转账");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    this.$store.commit("fanhui", false);
    if (this.$route.params.name != "no") {
      localStorage.setItem("phone", this.$route.params.name);
      this.num = localStorage.getItem("phone");
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  min-height: 100%;
  overflow-y: auto;
}
.div1 {
  margin: 0 auto;
  margin-top: 0.7rem;
  border-bottom: 1px solid #a5bce5;
  width: 82%;
}
.div1 > input {
  padding-left: 0.3rem;
  background: none;
  color: #fff;
  height: 0.9rem;
  width: 66%;
  font-size: 0.3rem;
}
.div1 > span {
  color: #fff;
  font-size: 0.3rem;
}
.div > button {
  height: 0.8rem;
  width: 90%;
  margin-top: 1.1rem;
  background: #e0a108;
  margin-left: 5%;
  border-radius: 5px;
}
.erwm {
  background: #fff;
}
.div2 {
  width: 80%;
  /* height: 50%; */
  /* background: #232323; */
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  /* margin-left: 10%; */
  border-radius: 0.1rem;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding-left: 0.3rem;
  padding-right: 0.7rem;
  color: #fff;
  z-index: 999;
  margin-bottom: 2rem;
}
.div2 > div {
  padding-top: 0.4rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid #fff;
}
.div2 > div > span {
  display: inline-block !important;
  width: 1.6rem;
}
.div2 > div > input {
  font-size: 0.3rem;
  width: 60%;
  background: none;
  color: #fff;
}
.btn {
  background: #e0a108;
  color: #fff;
  border-radius: 4px;
  padding-left: 0.2rem;
  padding-right: 0.2rem;
  margin-top: 0.2rem;
  margin-bottom: 0.6rem;
}
.btn1 {
  float: right;
}
.op {
  height: 100%;
  width: 100%;
  background: #fff;
  position: absolute;
  top: 0;
  opacity: 0.4;
}
</style>