<template>
  <div class="div">
    <p>HGH充币</p>
    <qriously class="img" :value="initQCode" :size="199"/>
    <!-- <p class="p" style="padding-bottom:0.3rem">长按二维码保存到相册</p> -->
    <textarea name id cols="30" rows="10" class="text" v-model="message" readonly="readonly"></textarea>
    <button
      type="button"
      v-clipboard:copy="message"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError"
    >复制地址</button>
  </div>
</template>
<script>
import VueClipboard from "vue-clipboard2";
export default {
  data() {
    return {
      initQCode: "",
      message: ""
    };
  },
  methods: {
    onCopy: function(e) {
      this.$toasted.success("复制成功").goAway(1200);
    },
    onError: function(e) {
      this.$toasted.error("Failed to copy texts").goAway(1200);
    }
  },
  mounted() {
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("size", "我的钱包");
    this.$store.commit("fanhui", true);
    const _this = this;
    _this.http
      .post("/api/center/hgh_address")
      .then(res => {
        if (res.code == 200) {
          console.log(res);
          _this.initQCode = res.data.src;
          _this.message = res.data.str;
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
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
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.div > p {
  font-size: 0.44rem;
  margin-top: 0.72rem;
  color: #fff;
  text-align: center;
}
.img {
  height: 4rem;
  width: 4rem;
  display: block;
  margin: 0 auto;
  margin-top: 0.66rem;
  background: #fff;
}
.p {
  font-size: 0.3rem !important;
  color: #0e66ad !important;
}
.p1 {
  height: 1rem;
  width: 90%;
  margin-left: 5%;
  color: #fff !important;
  font-size: 0.24rem !important;
  overflow: hidden;
}
.div > button {
  width: 90%;
  height: 0.8rem;
  background: #e0a108;
  margin-left: 5%;
  margin-top: 0.5rem;
  font-size: 0.34rem;
  color: #fff;
  border-radius: 5px;
}
.text {
  height: 1rem;
  width: 100%;
  padding-left: 0.8rem;
  background: none;
  background-size: cover;
  color: #fff;
  padding-right: 0.8rem;
  padding-top: 0.16rem;
  text-align: center;
}
</style>
