<template>
  <div class="div">
    <p class="p1" @click="touxiang()">
      <span>头像</span>
      <span class="span">
        <img class="img" :src="this.src" alt v-if="this.$store.state.Bool">
        <i class="iconfont icon6" style="float:right" v-if="!this.$store.state.Bool">&#xe61a;</i>
      </span>
    </p>
    <p class="p2">
      <span>用户名</span>
      <i class="iconfont icon4" @click="icon()" v-if="this.$store.state.Bool">&#xe615;</i>
      <span class="span1" v-if="this.$store.state.Bool">{{ name }}</span>
    </p>
    <p class="p2" @click="xiugai()">
      <span>银行卡绑定账户修改</span>
      <span>
        <i class="iconfont" style="float:right">&#xe604;</i>
      </span>
    </p>
    <!-- <p class="p2" @click="tab()">
      <span>Chinese-English shift/中英文切换</span>
      <span>
        <i class="iconfont" :class="{ivon:boll,ivon1:!boll}" style="float:right">&#xe604;</i>
      </span>
      <span class="div2" v-if="boll">
        <div class="div3"></div>
        <div class="div4" @click.stop="language()">
          <p class="p4" :class="{p6:this.$store.state.language}"></p>中文/Chinese
        </div>
        <div class="div4" @click="language1()">
          <p class="p4" :class="{p6:this.$store.state.language1}"></p>英文/English
        </div>
      </span>
    </p>-->
    <p class="p2" @click="gender()">
      <span>性别</span>
      <span>
        <i class="iconfont" style="float:right">&#xe604;</i>
      </span>
    </p>
    <p class="p2" @click="xg()">
      <span>修改密码</span>
      <span>
        <i class="iconfont" style="float:right">&#xe604;</i>
      </span>
    </p>
    <p class="p3" @click="zhifu1()">
      <span>支付密码</span>
      <span>
        <i class="iconfont" style="float:right">&#xe604;</i>
      </span>
    </p>
    <button @click="top()" class="llcy">安全退出</button>
  </div>
</template>
<script>
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      name: "",
      src: "",
      boll: false
    };
  },
  mounted() {
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("size", "个人中心");
    this.$store.commit("sidebar", false);
    this.$store.commit("tishi1", false);
    this.$store.commit("fanhui", true);
    this.$store.commit("Bool", false);
    if (localStorage.getItem("username")) {
      this.name = localStorage.getItem("name");
      this.src = localStorage.getItem("avatar");
      this.$store.commit("Bool", true);
    } else {
      this.name = "未登录";
      this.src = "&#xe61a;";
      this.$store.commit("Bool", false);
    }
  },
  methods: {
    //   退出的事件
    top() {
      this.$router.push("/");
      Toast({
        message: "退出成功",
        position: "bottom",
        duration: 1000
      });

      localStorage.clear();
    },
    icon() {
      this.$router.push("/nickname");
    },
    // 语言切换
    language() {
      this.boll = false;
      this.$store.commit("language", true);
      this.$store.commit("language1", false);
    },
    language1() {
      this.boll = false;
      this.$store.commit("language", false);
      this.$store.commit("language1", true);
    },
    tab() {
      this.boll = !this.boll;
    },
    xg() {
      this.$router.push("/xg/Setpassword");
    },
    zhifu1() {
      this.$router.push("/zhifu1/Paypassword");
    },
    xiugai() {
      this.$router.push("/ka/management");
    },
    touxiang() {
      this.$router.push("/touxiang/Modify");
    },
    // 跳转修改性别页面
    gender() {
      this.$router.push("/gender/Modify");
    }
  }
};
</script>
<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../../assets/font1/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.div {
  padding-top: 0.88rem;
  min-height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  overflow: hidden;
}
.p1 {
  height: 1.49rem;
  width: 100%;
  padding-left: 0.5rem;
  padding-right: 0.62rem;
  padding-top: 0.64rem;
  padding-bottom: 0.56rem;
  border-bottom: 1px solid #5b91eb;
}
.icon6 {
  position: absolute;
  top: 45%;
  left: 28%;
}
.p2 {
  padding-top: 0.4rem;
  padding-bottom: 0.5rem;
  padding-left: 0.5rem;
  padding-right: 0.62rem;
  border-bottom: 1px solid #5b91eb;
  position: relative;
}
.p3 {
  padding-top: 0.4rem;
  padding-bottom: 0.5rem;
  padding-left: 0.5rem;
  padding-right: 0.62rem;
  border-bottom: 1px solid #5b91eb;
}
.div > p {
  color: #fff;
  font-size: 0.3rem;
}
.span {
  line-height: 0;
  float: right;
  display: inline-block;
  height: 0.7rem;
  width: 0.7rem;
  border-radius: 50%;
  position: relative;
  text-align: center;
  overflow: hidden;
}
.span > img {
  position: absolute;
  top: 0;
  left: 0;
}
.icon4 {
  float: right;
  font-size: 0.3rem;
  color: #939caa;
}
.span1 {
  padding-right: 0.36rem;
}
.div2 {
  z-index: 111111;
  position: absolute;
  display: block;
  background: #296ec6;
  height: 2rem;
  width: 3.04rem;
  right: 0.22rem;
  top: 1rem;
}
.div3 {
  position: absolute;
  width: 0;
  height: 0;
  border-width: 0 0.14rem 0.26rem;
  border-style: solid;
  border-color: transparent transparent #296ec6;
  top: -0.26rem;
  right: 0.44rem;
}
.p4 {
  display: inline-block;
  height: 0.28rem;
  width: 0.28rem;
  border-radius: 50%;
  border: 1px solid #ffffff;
  margin: 0 0.22rem;
}
.div4 {
  padding-top: 0.4rem;
}
.p6 {
  background: #ebce5b;
  border: 0;
}
.ivon {
  transform: rotate(90deg);
  transition-duration: 1s;
}
.ivon1 {
  transform: rotate(0deg);
  transition-duration: 1s;
}
button {
  height: 0.82rem;
  width: 90%;
  background: #de9f27;
  margin-top: 0.88rem;
  margin-left: 5%;
  color: #fff;
  border-radius: 4px;
}
.span1 {
  float: right;
}
.img {
  height: 0.7rem;
  width: 0.7rem;
}
.llcy {
  margin-bottom: 2rem;
}
</style>