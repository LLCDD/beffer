<template>
  <div class="div">
    <div>
      <span @click="span()">
        <i class="iconfont" :class="{color:bool}">&#xe872;</i>
      </span>
      <span @click="span1()">
        <i class="iconfont" :class="{color:!bool}">&#xe873;</i>
      </span>
    </div>
    <div class="msg">{{ msg }}</div>
    <button @click="button()">确定修改</button>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      msg: "",
      bool: false,
      src: "",
      num: ""
    };
  },
  mounted() {
    var _this = this;
    this.$store.commit("size", "修改性别");
    var _this = this;
    _this.http.post("/api/center/gender").then(res => {
      if (res.code == 200) {
        console.log(res);
        if (res.data.gender.gender == "1") {
          _this.bool = false;
          _this.msg = "男";
        } else if (res.data.gender.gender == "2") {
          _this.bool = true;
          _this.msg = "女";
        }
      }
    });
  },
  methods: {
    span() {
      this.bool = true;
      this.msg = "女";
      this.num = "2";
    },
    span1() {
      this.bool = false;
      this.msg = "男";
      this.num = "1";
    },
    button() {
      var _this = this;
      _this.http
        .post("/api/center/gender_bind", { gender: _this.num })
        .then(res => {
          if (res.code == 200) {
            console.log(res);
            _this.$router.push("/center/personal");
            _this.$toasted.success("操作成功").goAway(1500);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
        });
    }
  }
};
</script>
<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../../assets/font4/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 2rem;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.div {
  padding-top: 0.88rem;
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
span {
  display: inline-block;
  /* padding-top: 1rem;
  padding-left: 1.2rem; */
  width: 50%;
  text-align: center;
  padding-top: 1rem;
}
.color {
  color: aquamarine;
}
.msg {
  height: 1rem;
  width: 1rem;
  margin: 0 auto;
  margin-top: 1rem;
  text-align: center;
  line-height: 1rem;
  color: #fff;
  font-size: 0.4rem;
}
button {
  height: 0.8rem;
  width: 90%;
  background: #de9f27;
  margin-left: 5%;
  color: #fff;
  border-radius: 4px;
  margin-top: 0.5rem;
}
</style>
