<template>
  <div class="theraise">
    <div class="div1">
      <p>
        <router-link tag="span" to="mai">买入</router-link>
      </p>
      <p>
        <router-link tag="span" to="chu">卖出</router-link>
      </p>
      <p>
        <router-link tag="span" to="dingdan">当前订单</router-link>
      </p>
      <p>
        <router-link tag="span" to="old">历史订单</router-link>
      </p>
      <p>
        <router-link tag="span" to="zhong">仲裁订单</router-link>
      </p>
    </div>
    <transition enter-active-class="animated fadeInLeft" leave-active-class="animated fadeOutRight">
      <router-view style=" position:absolute"></router-view>
    </transition>
  </div>
</template>

<script>
import { PullRefresh } from "vant";
export default {
  data() {
    return {
      msg: "343",
      isLoading: false
    };
  },
  mounted() {
    // this.$store.commit("size", this.$route.params.name);
    this.$store.commit("size", "HGH/CNY");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi1", true);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    this.$store.commit("fanhui", false);
  },
  methods: {
    onRefresh() {
      setTimeout(() => {
        this.$toast("刷新成功");
        this.isLoading = false;
      }, 500);
    }
  }
};
</script>

<style scoped>
.theraise {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  min-height: 100%;
  overflow: hidden;
}
.div1 {
  color: #fff;
}
.div1 > p {
  width: 19.9%;
  display: inline-block;
  font-size: 0.3rem;
  padding-top: 0.36rem;
  padding-bottom: 3px;
  text-align: center;
}
/* .div1 > span {
  display: inline-block;
  width: 19.9%;
  text-align: center;
  /* margin-left: 0.45rem; 
  font-size: 0.3rem;
  padding-top: 0.36rem;
  padding-bottom: 3px;
} */

.router-link-active {
  border-bottom: 2px solid #34ede6;
  color: #34ede6;
}
</style>
