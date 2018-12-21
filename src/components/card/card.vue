<template>
  <div class="div">
    <div class="div1"></div>
    <img class="img" src="../../assets/image/logo.png" alt>
    <h1 class="h1">扫码注册 HGH账号</h1>
    <qriously class="img1" :value="initQCode" :size="199"/>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "分享",
      initQCode: ""
    };
  },
  mounted() {
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("size", "分享");
    this.$store.commit("fanhui", true);
    if (this.$route.params.name != "no") {
      localStorage.setItem("phone", this.$route.params.name);
    }
    const _this = this;
    _this.http
      .post("/api/center/recommend")
      .then(res => {
        if (res.code == 200) {
          console.log(res);
          _this.initQCode = res.data.src;
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
      });
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
.div1 {
  margin: 0 auto;
  text-align: center;
}
.img {
  display: block;
  height: 2.28rem;
  width: 2.28rem;
  margin: 0 auto;
  margin-top: 0.88rem;
}
.h1 {
  text-align: center;
  font-size: 0.46rem;
  color: #fff;
  margin-top: 0.44rem;
}
.img1 {
  display: block;
  height: 4rem;
  width: 4rem;
  margin: 0 auto;
  margin-top: 0.96rem;
  background: #fff;
}
.div > p {
  text-align: center;
  color: #fff;
  font-size: 0.38rem;
  margin-top: 0.26rem;
}
</style>