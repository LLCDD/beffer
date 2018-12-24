<template>
  <div id="div1">
    <mt-swipe :auto="4000" style="height:3.4rem;margin-top:0.88rem">
      <mt-swipe-item>
        <img class="image" :src="imag[0].pic" alt>
      </mt-swipe-item>
      <mt-swipe-item>
        <img class="image" :src="imag[1].pic" alt>
      </mt-swipe-item>
      <mt-swipe-item>
        <img class="image" :src="imag[2].pic" alt>
      </mt-swipe-item>
      <mt-swipe-item>
        <img class="image" :src="imag[0].pic" alt>
      </mt-swipe-item>
    </mt-swipe>
    <div class="div2">
      <p class="p1" v-if="this.$store.state.tishi">HGH : 0.00</p>
      <p class="p1" v-if="!this.$store.state.tishi">Token :{{ token }}</p>
      <p class="p2" v-if="this.$store.state.tishi">会员ID:123456</p>
      <p class="p2" v-if="!this.$store.state.tishi">会员ID:{{ vip }}</p>
    </div>
    <p class="p3" v-if="this.$store.state.tishi">
      <span class="iconfont">&#xe60f;</span>新用户激活（不激活将无法操作大部分功能）
    </p>
    <div class="div5">
      <router-link to="/shop/assets" tag="div">
        <img class="img2" src="../../assets/icon/zichangguanli.png" alt>
        <br>
        <span>我得资产</span>
      </router-link>
      <router-link to="/code/announcement" tag="div">
        <img class="img2" src="../../assets/icon/gonggao.png" alt>
        <br>
        <span>公告</span>
      </router-link>
      <router-link to="/trading/AssetsReport" tag="div">
        <img class="img2" src="../../assets/icon/mingxi.png" alt>
        <br>
        <span>资产明细</span>
      </router-link>
      <router-link to="/theraise/trading/mai" tag="div">
        <img class="img2" src="../../assets/icon/C2C.png" alt>
        <br>
        <span>C2C</span>
      </router-link>
      <router-link to="/mall/address" tag="div">
        <img class="img2" src="../../assets/icon/qianbao.png" alt>
        <br>
        <span>钱包地址</span>
      </router-link>
      <router-link to="/pingtaizhifu" tag="div">
        <img class="img2" src="../../assets/icon/shouzhi.png" alt>
        <br>
        <span>平台收支</span>
      </router-link>
      <router-link to="/market/no" tag="div">
        <img class="img2" src="../../assets/icon/zhunanchu.png" alt>
        <br>
        <span>转出HGH</span>
      </router-link>
      <router-link to="/mall/address" tag="div">
        <img class="img2" src="../../assets/icon/jieshou.png" alt>
        <br>
        <span>接收HGH</span>
      </router-link>
      <router-link to="/card/share" tag="div">
        <img class="img2" src="../../assets/icon/fenxiang.png" alt>
        <br>
        <span>分享HGH</span>
      </router-link>
      <router-link to="/myhome" tag="div" v-if="!this.$store.state.tishi">
        <img class="img2" src="../../assets/icon/m.png" alt>
        <br>
        <span>im Token</span>
      </router-link>
      <div>
        <img class="img2 visi" src="../../assets/icon/m.png" alt>
        <br>
        <span class="visi">im Token</span>
      </div>
      <div>
        <img class="img2 visi" src="../../assets/icon/m.png" alt>
        <br>
        <span class="visi">im Token</span>

        <div class="ceshi1"></div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      msg: "这是主页",
      // 登录后的token值
      token: "",
      // 会员ID
      vip: "已登录",
      list: [
        // 自己模拟的数据
        {
          imag: "../../assets/img/1.png",
          txt: "从市场流通分析DTB走势",
          time: "2018-10-12",
          look: "800"
        },
        {
          imag: "../../assets/img/2.png",
          txt: "从市场流通分析DTB走势",
          time: "2018-10-13",
          look: "81231"
        }
      ],
      imag: JSON.parse(localStorage.getItem("img"))
    };
  },
  methods: {
    //   一键收取的事件
    top() {
      console.log("a");
    }
  },
  mounted() {
    this.$store.commit("fanhuiy", false);
    if (localStorage.getItem("img")) {
      this.imag = JSON.parse(localStorage.getItem("img"));
    }
    // 在获取一遍用户名
    // 获取 用户的信息
    this.$store.commit("headerStatus", true);
    this.$store.commit("tabBar", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("fanhui", false);
    this.$store.commit("tishi", false);
    this.$store.commit("user", localStorage.getItem("username"));
    const _this = this;
    _this.http.post("/api/center/index").then(res => {
      if (res.code == 200) {
        localStorage.setItem("img", JSON.stringify(res.data.banners));
        localStorage.setItem("avatar", res.data.user.avatar);
        localStorage.setItem("uid", res.data.user.id);
        localStorage.setItem("name", res.data.user.nickname);
        _this.$store.commit("hname", localStorage.getItem("name"));
        _this.token = res.data.user.wallet.hgh;
        _this.vip = res.data.user.wallet.user_id;
        _this.$store.commit("src", localStorage.getItem("avatar"));
      }
    });

    if (localStorage.getItem("username")) {
      this.$store.commit("tishi", false);
      this.$store.commit("Bool", true);
      this.touxiang = localStorage.getItem("avatar");
      this.$store.commit("Bool", true);
    } else {
      this.$store.commit("Bool", false);
    }
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
@font-face {
  font-family: "iconfont1";
  src: url("../../assets/font2/iconfont.woff") format("woff");
}
.iconfont1 {
  font-family: "iconfont1" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
#div1 {
  min-height: 100%;
  width: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
  overflow: hidden;
}
.img {
  width: 100%;
  margin-top: 0.86rem;
  height: 8.64rem;
  display: block;
}
.div2 {
  margin-top: -1px;
  height: 1.46rem;
  width: 100%;
  color: #ccc;
  text-align: center;
  border-bottom: 1px solid #2b89cb;
}
.p1 {
  font-size: 0.4rem;
  font-weight: 700;
  padding-top: 0.4rem;
  color: #69f4e5;
}
.p2 {
  font-size: 0.28rem;
  margin-top: 0.14rem;
  color: #4fb9ea;
}
.p3 {
  color: #ffffff;
  height: 0.6rem;
  width: 100%;
  background: #051533;
  line-height: 0.6rem;
  padding-left: 0.4rem;
}
.p3 > span {
  color: red;
  margin-right: 0.1rem;
}
.div4 > div {
  height: 1.42rem;
  width: 1.42rem;
  margin-top: 0.7rem;
  float: left;
  border-radius: 50%;
  overflow: hidden;
}
.div4 > div > img {
  width: 100%;
  height: 100%;
}
.div4 > button {
  border: 0;
  width: 1.4rem;
  border-radius: 0.2rem;
  margin-top: 0.36rem;
  background: #c2a47e;
  color: #fff;
  font-size: 0.22rem;
}
.div5 {
  margin: 0 auto;
  width: 100%;
  min-height: 5.68rem;
  overflow: hidden;
  margin-bottom: 1rem;
}
.div5 > div > i {
  font-size: 0.48rem;
}
.div5 > div > span {
  display: inline-block;
  margin-top: 0.16rem;
  color: #5d90e9;
}
.div5 > div {
  width: 33.3%;
  height: 33.3%;
  float: left;
  text-align: center;
  border-bottom: 1px solid #2b89cb;
  padding-top: 0.4rem;
  padding-bottom: 0.5rem;
}
.h3 {
  margin-top: 0.5rem;
  margin-left: 0.26rem;
  height: 0.4rem;
  font-size: 0.36rem;
  border-left: 0.1rem solid #c2a67f;
  margin-bottom: 0.32rem;
  color: #6a6a6a;
}
.span {
  margin-left: 0.1rem;
}
.span1 {
  margin-left: 0.1rem;
  font-size: 0.2rem;
}
.div6 {
  width: 100%;
  margin-bottom: 1rem;
}
.div6 > div > div {
  background: #fff;
  margin-bottom: 0.2rem;
  height: 1.76rem;
}
.div6 > div > div > img {
  height: 1.3rem;
  width: 1.9rem;
  float: left;
  border: 1px solid yellow;
  margin-top: 0.24rem;
  margin-left: 0.24rem;
  margin-right: 0.32rem;
}
.div6 > div > div > .h3l {
  font-size: 0.34rem;
  line-height: 1.1rem;
}
.div6 > div > div > .vspan1 {
  float: right;
  margin-right: 0.8rem;
}
.img2 {
  height: 0.6rem;
  width: 0.5rem;
}
.image {
  display: block;
  height: 100%;
  width: 100%;
}
.visi {
  visibility: hidden;
}
/* .ceshi1 {
  height: 100px;
  width: 2rem;
  background: red;
} */
</style>