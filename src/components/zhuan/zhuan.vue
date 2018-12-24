<template>
  <div class="div">
    <div>
      <p class="p1">
        <span class="span">{{ info.release_son }}</span>
        <br>
        <span class="span1">裂变子链</span>
      </p>
      <p>
        <span class="span">{{ info.hgh }}</span>
        <br>
        <span class="span1">HGH</span>
      </p>
    </div>
    <div class="div2">
      <p class="top">兑换数量</p>
      <input class="top top1" type="text" v-model="yeuhuan" placeholder="请输入购买数量">
    </div>
    <div class="div2">
      <p class="top">支付密码</p>
      <input class="top top1" type="password" placeholder="请输入支付密码" v-model="password">
    </div>
    <div class="div2">
      <p class="top">应得HGH</p>
      <span class="top">{{ hgh1 }}</span>
    </div>
    <button @click="top()">确定兑换</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      yeuhuan: "",
      password: "",
      info: ""
    };
  },
  computed: {
    shouru() {
      return this.yeuhuan * 1 + ".00";
    },
    hgh1() {
      return this.yeuhuan * 1;
    }
  },
  mounted() {
    this.$store.commit("fanhui", true);
    this.$store.commit("size", "裂变子链转HGH");
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    // console.log(this.$store.state.info[1].code);
    const _this = this;
    _this.http
      .post("/api/asset/dynamic_exc")
      .then(res => {
        if (res.code == 200) {
          // console.log(res);
          _this.info = res.data.wallet;
          console.log(_this.info);
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
      });
  },
  methods: {
    top() {
      if (this.yeuhuan != "" || this.password != "") {
        const _this = this;
        _this.http
          .post("/api/asset/exc_make", {
            num: _this.yeuhuan,
            security: _this.password,
            type: 1
          })
          .then(res => {
            if (res.code == 200) {
              _this.$router.push("/shop/assets");
              _this.$toasted.success("操作成功").goAway(1400);

              _this.$router.push("/shop/assets");
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(1400);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1400);
          });
      } else {
        this.$toasted.error("请填写完整信息", { icon: "error" }).goAway(1500);
      }
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  min-height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.div > div {
  width: 100%;
  border-bottom: 1px solid #5b91eb;
  margin-top: 0.24rem;
  float: left;
}
.div > div > p {
  float: left;
  width: 49.6%;
  text-align: center;
}
.span {
  padding-top: 0.26rem;
  display: inline-block;
  color: #fff;
  font-size: 0.26rem;
  margin-bottom: 0.22rem;
}
.span1 {
  display: inline-block;
  color: #c0c4cd;
  padding-bottom: 0.28rem;
}
.p1 {
  border-right: 1px solid #5b91eb;
}
.div2 {
  color: #fff;
}
.top {
  display: inline-block;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
  color: #fff;
  width: 40%;
}
.top1 {
  background: none;
}
button {
  width: 90%;
  height: 0.8rem;
  margin-left: 5%;
  margin-top: 1.18rem;
  background: #de9f27;
  border-radius: 4px;
  color: #fff;
  font-size: 0.3rem;
  margin-bottom: 1rem;
}
</style>