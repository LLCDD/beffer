<template>
  <div class="div" @click="div()">
    <p>
      <span>地&nbsp;&nbsp;址：</span>
      <input type="text" v-model="name" placeholder="请输入地址">
    </p>

    <p class="p">
      <span>收&nbsp;款&nbsp;码：</span>
      
      <span @click="shangchuan()">
        <mt-button size="small" type="primary">上传</mt-button>
      </span>
      <input
        type="file"
        class="uploadphoto"
        @change="uploadphoto($event)"
        ref="inputer"
        multiple
        accept="image/*"
      >
    </p>
    <span class="spany">
      <img class="dimg" :src="src" alt>
    </span>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "支付宝",
      name: "",
      src: "",
      src1: ""
    };
  },
  methods: {
    shangchuan() {
      this.style = true;
    },
    div() {
      this.style = false;
    },
    // 生成base64的字符
    uploadphoto(e) {
      var _this = this;
      var file = e.target.files[0];
      var filesize = file.size;
      var filename = file.name;
      // 2,621,440   2M
      var reader = new FileReader();
      reader.readAsDataURL(file);
      const _this = this;
      reader.onload = function(e) {
        // 读取到的图片base64 数据编码 将此编码字符串传给后台即可
        var img = e.target.result;
        _this.http
          .post("/api/center/imtoken_bind", {
            img: img,
            address: _this.name
          })
          .then(res => {
            if (res.code == 200) {
              _this.http.post("/api/center/imtoken").then(res => {
                if (res.code == 200) {
                  if (res.data.info.imtoken == null) {
                    _this.name = _this.src = "";
                  } else {
                    _this.name = res.data.info.imtoken;
                    _this.src = res.data.info.imtoken_img;
                  }
                  _this.$router.push("/zhifu/management");
                }
              });
              _this.$toasted.success("操作成功").goAway(2000);
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          });
      };
    }
    // -----------------------------------------
  },
  mounted() {
    this.$store.commit("size", "MyToken");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("fanhui", false);
    this.$store.commit("fanhui", true);
    const _this = this;
    _this.http
      .post("/api/center/imtoken")
      .then(res => {
        if (res.code == 200) {
          if (res.data.info.imtoken == null) {
            _this.name = _this.src = "";
          } else {
            _this.name = res.data.info.imtoken;
            _this.src = res.data.info.imtoken_img;
          }
        } else if (res.code == 400) {
          this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
      });
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  min-height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  color: #fff;
  overflow: hidden;
}
.div > p {
  padding-top: 0.76rem;
  padding-left: 0.76rem;
  font-size: 0.3rem;
}
.div > p > span {
  display: inline-block;
  width: 1.76rem;
}
.div > p > input {
  background: none;
  color: #fff;
}
.uploadphoto {
  opacity: 1;
  position: absolute;
  left: 2.5rem;
  z-index: 99;
  opacity: 0;
}
.p {
  position: relative;
}
.dimg {
  display: block;
  height: 100%;
  width: 100%;
  margin-top: 0.4rem;
}
.spany {
  display: inline-block;
  width: 80%;
  margin-left: 10%;
  padding-bottom: 1rem;
}
</style>