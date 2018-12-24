<template>
  <div class="div">
    <div class="div1">
      <img class="img" :src="src" alt>
      <!-- <img :src="src" alt> -->
      <i v-if="!this.bool" class="iconfont">&#xe873;</i>
    </div>
    <button>修改头像</button>
    <input
      type="file"
      class="uploadphoto"
      @change="uploadphoto($event)"
      ref="inputer"
      multiple
      accept="image/*"
    >
  </div>
</template>
<script>
import { PullRefresh } from "vant";
export default {
  data() {
    return {
      msg: "修改头像",
      bool: true,
      src: localStorage.getItem("avatar")
    };
  },
  mounted() {
    this.$store.commit("size", "修改头像");
    if (localStorage.getItem("avatar")) {
      this.bool = true;
    } else {
      this.bool = !this.bool;
    }
  },
  methods: {
    uploadphoto(e) {
      var file = e.target.files[0];
      var filesize = file.size;
      var filename = file.name;
      // 2,621,440   2M
      var reader = new FileReader();
      reader.readAsDataURL(file);
      const _this = this;
      reader.onload = function(e) {
        // 读取到的图片base64 数据编码 将此编码字符串传给后台即可
        var imgcode = e.target.result;
        _this.http
          .post("/api/center/change_head", { img: imgcode })
          .then(res => {
            if (res.code == 200) {
              // _this.$store.commit("setAvatar", res.data.avatar);
              localStorage.setItem("avatar", res.data.img);
              _this.src = res.data.img;
              _this.$toasted.success("操作成功").goAway(1500);
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          });
      };
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
.div1 {
  height: 4rem;
  width: 4rem;
  margin: 0 auto;
  border-radius: 50%;
  margin-top: 1rem;
  overflow: hidden;
  text-align: center;
  line-height: 4rem;
}
button {
  width: 90%;
  margin-left: 5%;
  margin-top: 1rem;
  height: 0.8rem;
  background: #de9f27;
  border-radius: 4px;
  color: #ffffff;
}
.uploadphoto {
  margin-top: -1rem;
  background: red;
  width: 90%;
  margin-left: 5%;
  height: 0.8rem;
  opacity: 0;
}
.img {
  height: 4rem;
  width: 4rem;
}
</style>
