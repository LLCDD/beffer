<template>
  <div class="div">
    <p>
      <router-link tag="span" to="quanbu">全部</router-link>
    </p>
    <p>
      <router-link tag="span" to="zhuanqu">赚取</router-link>
    </p>
    <p>
      <router-link tag="span" to="zhichu">支出</router-link>
    </p>
    <transition enter-active-class="animated fadeInLeft" leave-active-class="animated fadeOutRight">
      <router-view style=" position: absolute"></router-view>
    </transition>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "我的交易"
    };
  },
  mounted() {
    this.$store.commit("size", "资产明细");
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("fanhui", false);
    this.$store.commit("fanhuiy", true);
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  min-height: 100%;
  width: 100%;
  overflow: hidden;
}
.div > p {
  width: 33.3%;
  display: inline-block;
  text-align: center;
  padding: 0 0.5rem;
  margin-top: 0.54rem;
  padding-bottom: 0.16rem;
  color: #ffffff;
}

.div > span {
}
.router-link-active {
  color: #27e3d8;
  border-bottom: 2px solid #27e3d8;
}
</style>
