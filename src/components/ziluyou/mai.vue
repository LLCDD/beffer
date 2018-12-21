<template>
  <div class="div1">
    <div>
      <span class="span1">买入估价：</span>
      <span>{{ msg }}</span>
    </div>
    <div>
      <span class="span1">买入数量：</span>
      <input v-model="num" class="input" type="text" placeholder="请输入数量">
      <!-- <span>HGH</span> -->
    </div>
    <div>
      <span class="span1">兑换基数：</span>
      <span>1000</span>
    </div>
    <div>
      <span class="span1">买入金额：</span>
      <span>{{ sum }}</span>
    </div>
    <button @click="top()">发布</button>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      // 买入的估价
      msg: "0.00",
      num: ""
    };
  },
  computed: {
    sum() {
      return (this.msg * this.num).toFixed(2);
    }
  },
  methods: {
    top() {
      const _this = this;
      _this.http
        .post("/api/deal/buy_make", { num: _this.num })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success(res.message).goAway(1500);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
        });
    }
  },
  mounted() {
    this.$store.commit("tabBar", true);
    this.$store.commit("tishi1", true);
    this.$store.commit("fanhui", false);
    const _this = this;
    _this.http.post("/api/deal/index").then(res => {
      if (res.code == 200) {
        _this.msg = res.data.price;
      }
    });
  }
};
</script>
<style scoped>
.div1 {
  padding-top: 0.22rem;
  width: 100%;
}
.div1 > div {
  width: 100%;
  height: 1.08rem;
  border-bottom: 1px solid #5b91eb;
  font-size: 0.3rem;
  padding-left: 0.4rem;
  line-height: 1.08rem;
}
.div1 > button {
  width: 90%;
  margin-left: 5%;
  margin-top: 1.12rem;
  height: 0.82rem;
  background: #de9f27;
  color: #fff;
  font-size: 0.34rem;
}
.span1 {
  margin-right: 0.9rem;
}
.input {
  width: 3rem;
  border: 0;
  background: none;
  color: #fff;
}
</style>