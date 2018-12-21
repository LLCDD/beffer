<template>
  <div class="div">
    <div class="div1">
      <span>修改呢称</span>
      <input type="text" placeholder="请输入你要修改的呢称" v-model="num">
    </div>
    <button style="color:#fff" @click="top()">确定修改</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "交易",
      num: ""
    };
  },
  methods: {
    top() {
      if (this.num == "") {
        this.$toasted.error("账号不能为空", { icon: "error" }).goAway(2000);
      } else {
        const _this = this;
        _this.http
          .post("/api/center/nickname_bind", {
            nickname: _this.num
          })
          .then(res => {
            if (res.code == 200) {
              _this.$toasted.success("修改成功").goAway(1200);
              localStorage.setItem("name", _this.num);
              this.$store.commit("hname", _this.num);
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "eror" }).goAway(1200);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "eror" }).goAway(1200);
          });
      }
    }
  },
  mounted() {
    if (localStorage.getItem("name")) {
      this.num = localStorage.getItem("name");
    }
    this.$store.commit("tishi1", false);
    this.$store.commit("size", "呢称修改");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("fanhui", true);
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
  margin-top: 0.7rem;
  border-bottom: 1px solid #a5bce5;
  width: 82%;
}
.div1 > input {
  padding-left: 0.3rem;
  /* background: url("../../assets/image/500585755_banner.png"); */
  color: #fff;
  height: 0.9rem;
  width: 74%;
  font-size: 0.3rem;
  background: none;
}
.div1 > span {
  color: #fff;
  font-size: 0.3rem;
}
.div > button {
  height: 0.8rem;
  width: 90%;
  margin-top: 1.1rem;
  background: #e0a108;
  margin-left: 5%;
  border-radius: 5px;
}
.erwm {
  background: #fff;
}
.div2 {
  width: 80%;
  height: 40%;
  background: #232323;
  margin-left: 10%;
  border-radius: 0.1rem;
  position: absolute;
  top: 20%;
  padding-left: 0.3rem;
  padding-right: 0.7rem;
  color: #fff;
}
.div2 > div {
  padding-top: 0.4rem;
  padding-bottom: 0.4rem;
}
.div2 > div > span {
  display: inline-block !important;
  width: 1.6rem;
}
.div2 > div > input {
  font-size: 0.3rem;
}
.btn {
  float: left;
  background: #e0a108;
  color: #fff;
  border-radius: 4px;
  padding-left: 0.2rem;
  padding-right: 0.2rem;
}
.btn1 {
  float: right;
}
</style>