<template>
  <div class="div">
    <div></div>
    <p v-if="bool">您还没有绑定银行卡快去绑定版</p>
    <router-link to="/ka/management" tag="button" v-if="bool">添加</router-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "分享",
      bool: "false"
    };
  },
  mounted() {
    const _this = this;
    _this.http
      .post("/api/center/bank")
      .then(res => {
        if (res.code == 200) {
          console.log(res.data.info);
          if (res.data.info.bamk_num == "null") {
            _this.bool = false;
          } else {
            _this.bool - true;
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
button {
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
}
</style>