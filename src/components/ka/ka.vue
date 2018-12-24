<template>
  <div class="div">
    <p class="p1">只能绑定借记卡,充值提现时银行等姓名信息必修与您的认证名字一致</p>
    <p class="p2">
      持卡人姓名:
      <input type="text" placeholder="请输入姓名" v-model="name">
    </p>
    <p class="p2">
      卡属银行:
      <input type="text" placeholder="请输入卡属银行" v-model="hang">
    </p>
    <p class="p2">
      支行:
      <input type="text" placeholder="请输入支行" v-model="zhihang">
    </p>
    <p class="p2">
      银行卡账号:
      <input type="text" placeholder="请输入银行卡号" v-model="zhanghao">
    </p>
    <p class="p3">
      风险提示：C2C属于场外交易, 法币不经过平台, 平台不会自动扣款需要您手动自行转账给对方,
      对方才能收到款项绑定银行卡等收款信息只为核对信息之用, 不是扣款依据
    </p>
    <p class="i">绑定或修改代表您已阅读并充分了解上述内容, 由于款项造成的交易风险自行承担</p>
    <!-- <button @click="gai()">修改</button> -->
    <button @click="bind()">
      <span v-if="bool">添加</span>
      <span v-if="!bool">修改</span>
    </button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      hang: "",
      zhihang: "",
      zhanghao: "",
      // 添加还是修改的判断
      bool: true
    };
  },
  methods: {
    bind() {
      if (!this.name || !this.hang || !this.zhihang || !this.zhanghao) {
        this.$toasted.error("请填写完整信息", { icon: "error" }).goAway(1500);
      } else {
        const _this = this;
        _this.http
          .post("/api/center/bank_bind", {
            name: _this.name,
            type: _this.hang,
            address: _this.zhihang,
            num: _this.zhanghao
          })
          .then(res => {
            if (res.code == 200) {
              // console.log(res.data);
              _this.$router.push("/zhifu/management");
              _this.$toasted.success("操作成功").goAway(1500);
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
          });
      }
    }
  },
  mounted() {
    this.$store.commit("size", "支付管理");
    const _this = this;
    _this.http
      .post("/api/center/bank")
      .then(res => {
        if (res.code == 200) {
          console.log(res.data);
          if (res.data.info.bank_user == "null") {
            _this.bool = true;
          } else {
            _this.bool = false;
            _this.name = res.data.info.bank_user;
            _this.hang = res.data.info.bank_type;
            _this.zhihang = res.data.info.bank_address;
            _this.zhanghao = res.data.info.bank_num;
          }
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
      });
  }
};
</script>
<style scoped>
.div {
  min-height: 100%;
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.p1 {
  color: #9da4b1;
  padding: 0.18rem 0.36rem;
  line-height: 0.42rem;
  font-size: 0.28rem;
}
.div > p {
  border-bottom: 1px solid #5b91eb;
  padding-left: 0.36rem;
}
.p2 {
  color: #ffffff;
  padding-top: 0.3rem;
  padding-bottom: 0.3rem;
  font-size: 0.3rem;
}
.p2 > input {
  border: 0;
  background: none;
  color: #fff;
  width: 66%;
}
.p3 {
  line-height: 0.42rem;
  padding: 0.56rem 0.26rem 0.24rem 0.22rem;
  color: #5e8bb3;
  border-bottom: 0 !important;
}
.i {
  font-style: italic;
  color: red;
  padding-left: 0.22rem;
  line-height: 0.42rem;
  padding-right: 0.6rem;
  font-weight: 100;
  border-bottom: 0 !important;
}
.div > button {
  border: 0;
  width: 89.3%;
  margin-left: 5%;
  margin-top: 0.54rem;
  height: 0.82rem;
  border-radius: 4px;
  font-size: 0.32rem;
  color: #fff;
  background: #de9f27;
  font-weight: 100;
  margin-bottom: 2rem;
}
</style>