<template>
  <div class="div">
    <span class="llc" v-if="bool3">
      <img class="img3" :src="src" @click="tu()">
    </span>
    <div class="div1" :class="{div11:bool}">
      <p style="padding-top:0.5rem">
        <span>订单详情：</span>
        {{ detailas.deal.no }}
      </p>
      <p>
        <span>订单数量：</span>
        {{ detailas.deal.num }}
      </p>
      <p>
        <span>订单价格：</span>
        {{ detailas.deal.price }}
      </p>
      <p>
        <span>更新时间：</span>
        {{ detailas.deal.created_at }}
      </p>
      <p>
        <span>订单类型：</span>
        {{ state }} {{ states }}
      </p>
      <div class="div3">
        <button v-if="stateb" @click="cancel()">取消</button>
        <!-- <button >等于1 status 判断 待打款还是 却认打款</button> -->
        <button v-if="state1A">等待打款</button>
        <button v-if="state1">确认打款</button>

        <!-- <button >等于2 stateus 是等待收款 和 确认收款 and 投诉 查看图片</button> -->
        <button v-if="state2d" @click="other()">对方信息</button>
        <button v-if="state2C" @click="complaint_make()">投诉</button>
        <button v-if="state2" @click="look()">查看图片</button>
        <button v-if="state2B" @click="confirm()">确认收款</button>
        <button v-if="state2A">等待 收款</button>
        <div class="div4" style="padding-top:0.3rem" v-if="password">
          请输入支付密码
          <br>
          <input type="password" class="input" style="color:#000" v-model="input">
          <br>
          <button @click="quxiao()" style="float:left ;margin-left:0.2rem">取消</button>
          <button @click="yes()" style="float:right;margin-right:0.3rem">确定</button>
        </div>

        <input
          v-if="state1y"
          type="file"
          class="uploadphoto"
          @change="uploadphoto($event)"
          ref="inputer"
          multiple
          accept="image/*"
        >
      </div>
      <img v-if="img" :src="imgSrc" class="bug" alt @click="top()">
    </div>
    <div class="div2" v-if="boolllc">
      确定取消
      <button class="button" @click="cancel1()">取消</button>
      <button class="button button1" @click="cancel2()">确定</button>
    </div>
    <div class="div0" v-if="bool1">
      <!-- <div class="div0" v-if="true"> -->
      <textarea name id cols="30" rows="10" placeholder="请输入你要说的内容" v-model="text"></textarea>
      <button class="btn01 btn0" @click="disappear()">取消</button>
      <button class="btn0" @click="submit()">提交</button>
    </div>
    <!-- 用户详情 -->
    <div v-if="bool" class="div7" :class="{wei:bool12}">
      <i class="iconfont icony" @click="icon()">&#xe615;</i>
      <div class="divl">
        <div class="div9">
          <span class="lpan">用户名：{{ obj.other.nickname }}</span>
          <div class="div8">
            <img :src="obj.other.avatar" alt>
          </div>
        </div>
        <br>
      </div>
      <div class="div10">
        <p class="p1">对方支付宝信息</p>
        <span class="spanc">支付宝用户名：{{ obj.other.info.alipay_name }}</span>
        <br>
        <span class="spanc">支付宝账号:{{ obj.other.info.alipay_phone }}</span>
        <span class="spanc">
          支付宝收款码：
          <button class="butt" @click="screenshots()">点击查看收款码</button>

          <!-- <img class="img2" :src="obj.other.avatar" alt> -->
        </span>
      </div>
      <div class="div10">
        <p class="p1">对方微信支付信息</p>
        <span class="spanc">微信用户名:{{ obj.other.info.wechat_name }}</span>
        <br>
        <span class="spanc">微信宝账号:{{ obj.other.info.wechat_phone }}</span>
        <span class="spanc">
          微信收款码：
          <button class="butt" @click="screenshots1()">点击查看收款码</button>
          <br>
          <!-- <img class="img2" :src="obj.other.avatar" alt> -->
        </span>
      </div>
      <div class="div10">
        <p class="p1">对方imToken信息</p>
        <span class="spanc">imToken用户名:{{ obj.other.info.imtoken }}</span>
        <br>
        <span class="spanc">
          imToken收款码：
          <button class="butt" @click="screenshots2()">点击查看收款码</button>
          <br>
          <!-- <img class="img2" :src="obj.other.avatar" alt> -->
        </span>
      </div>
      <div class="div10 div5">
        <p class="p1">对方银行卡信息</p>
        <span class="spanc">银行卡用户名:{{ obj.other.info.bank_user }}</span>
        <br>
        <span class="spanc">开户支行:{{ obj.other.info.bank_type }}</span>
        <span class="spanc">银行卡账号:{{ obj.other.info.bank_num }}</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "order details",
      detailas: {
        deal: {
          id: 12,
          user_id: 63,
          price: "",
          num: "",
          status: 0,
          type: 1,
          created_at: "",
          no: "",
          updated_at: "",
          deal_id: null,
          img: null,
          fee: 10,
          fee1: 5,
          pay_at: null,
          confirm_at: null,
          deleted_at: null,
          puser: {
            id: 63,
            nickname: "初始账号",
            avatar:
              "http://192.168.0.189/uploads/head_img/20181217/5c16f845de4d6.jpeg"
          },
          auser: []
        },
        login_id: 63
      },
      img: false,
      imgSrc: "",
      states: "",
      state1: false,
      state2: false,
      state1A: false,
      state2A: false,
      state2B: false,
      state1y: false,

      stateb: true,
      // 判断时候点击取消出现的样式和功能
      bool: false,
      input: "",
      password: false,
      // text的内容

      text: "",
      // 第一次修改
      bool1: false,

      state2d: false,
      //投诉
      state2C: true,
      // 对方信息
      bool12: false,
      bool3: false,
      // asdasd
      boolllc: false
    };
  },
  computed: {
    state() {
      if (this.detailas.deal.type == 1) {
        if (this.detailas.deal.user_id == this.detailas.login_id) {
          return "买入";
        } else {
          return "卖出";
        }
      } else if (this.detailas.deal.type == "2") {
        if (this.detailas.deal.user_id == this.detailas.login_id) {
          return "卖出";
        } else {
          return "买入";
        }
      }
    }
  },
  methods: {
    look() {
      if (this.detailas.deal.img) {
        this.img = true;
        this.imgSrc = this.detailas.deal.img;
      } else {
        this.$toasted.error("还有没上传图片", { icon: "error" }).goAway(1200);
      }
    },
    // 取消的接口
    cancel() {
      this.boolllc = true;
    },
    cancel1() {
      this.boolllc = false;
    },
    // 确定取消
    cancel2() {
      const _this = this;
      _this.http
        .post("/api/deal/cancel", { id: _this.$route.params.id })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success("取消成功").goAway(1200);
            _this.$router.push("/theraise/trading/dingdan");
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });
    },
    // 对方的详情

    other() {
      this.bool12 = true;
      const _this = this;
      _this.http
        .post("/api/deal/other", { id: _this.$route.params.id })
        .then(res => {
          if (res.code == 200) {
            _this.obj = res.data;
            // console.log(res);
            _this.bool = true;
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
        });
    },
    icon() {
      this.bool = false;
      console.log(this.obj.other.info.alipay_name);
    },
    // 支付宝的支付图片
    screenshots() {
      console.log("支付宝");

      if (this.obj.other.info.wechat_img == null) {
        this.$toasted.error("还没有截图", { icon: "error" }).goAway(1200);
      } else {
        this.src = this.obj.other.info.alipay_img;
        this.bool3 = true;
      }
    },
    // 微信的支付截图
    screenshots1() {
      console.log("微信");
      if (this.obj.other.info.wechat_img == null) {
        this.$toasted.error("还没有截图", { icon: "error" }).goAway(1200);
      } else {
        this.src = this.obj.other.info.wechat_img;
        this.bool3 = true;
      }
    },
    // imToken 的支付截图
    screenshots2() {
      console.log("intoken");
      if (this.obj.other.info.imtoken_img == null) {
        this.$toasted.error("还没有截图", { icon: "error" }).goAway(1200);
      } else {
        this.src = this.obj.other.info.imtoken_img;
        this.bool3 = true;
      }
    },
    // 等待打款的打款处理
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
          .post("/api/deal/pay", { img: imgcode, id: _this.$route.params.id })
          .then(res => {
            if (res.code == 200) {
              _this.$toasted.success("操作成功").goAway(1500);
              history.go(0);
            } else if (res.code == 400) {
              _this.$toasted
                .error(res.data.message, { icon: "error" })
                .goAway(2000);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          });
      };
    },
    // 确认收款
    confirm() {
      this.password = true;
    },
    // 取消收款
    quxiao() {
      this.password = false;
      this.input = "";
    },
    // 再次确认付款发起ajax
    yes() {
      const _this = this;
      _this.http
        .post("/api/deal/confirm", {
          id: _this.$route.params.id,
          security: _this.input
        })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success(res.message).goAway(1200);
            _this.$router.push("/theraise/trading/dingdan");
            // history.go(0);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });
    },
    // 订单的投诉
    complaint_make() {
      this.bool1 = true;
    },
    //关闭投诉
    disappear() {
      this.bool1 = false;
      this.text = "";
    },
    // 提交
    submit() {
      const _this = this;

      _this.http
        .post("/api/deal/complaint_make", {
          id: _this.$route.params.id,
          text: _this.text
        })
        .then(res => {
          if (res.code == 200) {
            _this.bool1 = false;
            _this.text = "";
            _this.$router.push("/theraise/trading/dingdan");
            _this.$toasted.success(res.message).goAway(1200);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });
    },
    top() {
      this.img = false;
    },
    tu() {
      this.bool3 = false;
    }
  },
  mounted() {
    this.$store.commit("fanhui", true);
    this.$store.commit("size", "订单详情");
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    const _this = this;
    _this.http
      .post("/api/deal/detail", { id: _this.$route.params.id })
      .then(res => {
        if (res.code == 200) {
          _this.detailas = res.data;
          // console.log(_this.detailas);
          // 先判断他的type类型
          //   如果他的type类型是1的话是挂买的
          //   如果 他的type类型是2的话是挂卖
          //   如果 他的status等于0的话显示一个取消按钮  然后 状态 是为匹配
          //   如果  他的status 等于1的话  判断他的user和login 的值 给出待打款和确认打款
          //   如果  他的status 等于2的话  是等待收款  和  确认收款 和投诉  和  查看照片

          if (_this.detailas.deal.type == 1) {
            if (_this.detailas.deal.status == 0) {
              _this.states = "(待匹配)";
              //   出现一个取消按钮
              _this.stateb = true;
            } else if (_this.detailas.deal.status == 1) {
              if (_this.detailas.deal.user_id == _this.detailas.login_id) {
                // 确认打款
                _this.stateb = false; //取消
                _this.state1A = false; //等待打款
                _this.state1 = true; // 确认打款
                _this.state1y = true;
                _this.state2d = true; //对方信息
              } else {
                // 等待打款

                _this.stateb = false; //取消
                _this.state1A = true; //等待打款
                _this.state1 = false; //确认打款
                _this.state2d = true; //对方信息
              }
            } else if (_this.detailas.deal.status == 2) {
              _this.state2 = true;
              if (_this.detailas.deal.user_id == _this.detailas.login_id) {
                _this.state2A = true; //等待收款
                _this.staet2 = true; //打款截图
                _this.stateb = false; //取消
                _this.state2d = true; //对方信息
                // 等待收款
                // 看上传的图片
              } else {
                //  确认收款
                // 看上传的图片
                // 投诉
                _this.state2A = false; //等待收款
                _this.state2B = true; //确认收款
                _this.staet2 = true; //打款截图
                _this.stateb = false; //取消
                _this.state2d = true; //对方信息
              }
            }
          } else {
            if (_this.detailas.deal.status == 0) {
              _this.states = "(待匹配)";
              //   出现一个取消按钮
              _this.stateb = true;
            } else if (_this.detailas.deal.status == 1) {
              if (_this.detailas.deal.user_id == _this.detailas.login_id) {
                // 等待打款
                _this.stateb = false; //取消
                _this.state1A = true; //等待打款
                _this.state1 = false; //确认打款
                _this.state2d = true; //对方信息
              } else {
                // 确认打款
                _this.stateb = false; //取消
                _this.state1A = false; //等待打款
                _this.state1 = true; // 确认打款
                _this.state1y = true;
                _this.state2d = true; //对方信息
              }
            } else if (_this.detailas.deal.status == 2) {
              _this.state2 = true;
              if (_this.detailas.deal.user_id == _this.detailas.login_id) {
                _this.state2A = false; //等待收款
                _this.state2B = true; //确认收款
                _this.staet2 = true; //打款截图
                _this.stateb = false; //取消
                _this.state2d = true; //对方信息

                //  确认收款
                //   投诉
                // 看上传的图片
              } else {
                // 等待收款
                // 看上传的图片
                _this.state2A = true; //等待收款
                _this.staet2 = true; //打款截图
                _this.stateb = false; //取消
                _this.state2d = true; //对方信息
              }
            }
          }
          // 订单状态的处理
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
      });
    if (localStorage.getItem("zhong") == this.$route.params.id) {
      this.state2C = false;
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
  color: #fff;
  position: fixed;
  min-height: 100%;
  width: 100%;
  overflow-y: auto;
}

p {
  padding-left: 0.4rem;
  padding-top: 0.3rem;
  padding-bottom: 0.24rem;
  font-size: 0.3rem;
  border-bottom: 1px solid #5e85d2;
}
span {
  display: inline-block;
  width: 2rem;
}
button {
  width: 20%;
  margin-left: 2.2%;
  margin-top: 0.4rem;
  background: #de9f27;
  color: #fff;
  border-radius: 4px;
}
img {
  margin-top: 0.1rem;
  width: 100%;
  height: 3rem;
}
.div2 {
  position: absolute;
  top: 45%;
  left: 28%;
  width: 50%;
  min-height: 2rem;
  background: #232322;
  border-radius: 4px;
  text-align: center;
  font-size: 0.4rem;
  line-height: 1.3rem;
}
.button {
  position: absolute;
  bottom: 0;
  right: 0.1rem;
  font-size: 0.3rem;
  width: 1rem;
  background: #ccc;
  color: #000;
}
.button1 {
  left: 0;
  background: #de9f27;
  color: #fff;
}
.div11 {
  opacity: 0.3;
}
.uploadphoto {
  position: absolute;
  left: 0;
  top: 0.32rem;
  opacity: 0;
  width: 1.4rem;
}
.div3 {
  position: relative;
}
.div4 {
  position: fixed;

  height: 3rem;
  text-align: center;
  background: #ccc;
  font-size: 0.4rem;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 4px;
}
.input {
  letter-spacing: 0.1rem;
  margin-top: 0.4rem;
}
textarea {
  width: 90%;
  margin-bottom: 0.1rem;
  margin-left: 5%;
  margin-top: 0.2rem;
  color: #000;
}
.div0 {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  /* margin-top: 0.3rem; */
  /* margin-left: 10%; */
  background: #232323;
  /* float: left; */
  border-radius: 0.1rem;
  /* margin-bottom: 2rem; */
}
.btn0 {
  padding: 0;
  margin: 0;
  margin-right: 5%;
  float: right;
}
.btn01 {
  padding: 0;
  margin: 0;
  float: left;
  margin-left: 5%;
}
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
.div7 {
  width: 80%;
  background: #323433;
  background-size: cover;
  margin-left: 10%;
  float: left;
  margin-top: 0.3rem;
  padding-top: 0.3rem;
  padding-left: 0.3rem;
  padding-right: 0.6rem;
  position: absolute;
  top: 5%;
  border-radius: 0.1rem;
  margin-bottom: 1rem;
}
.divl {
  /* float: left; */
}
.div8 {
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  overflow: hidden;
  float: right;
  position: relative;
}
.div8 > img {
  display: block;
  height: 100%;
  width: 100%;
  position: absolute;
  top: -5px;
}
.div9 {
  /* padding-top: 0.4rem; */
  width: 100%;
  /* float: left; */
  background: #de9f27;
}
.div4 {
  background: #323433;
  /* font-size: 0.3rem; */
}
.p1 {
  padding: 0;
  border: 0;
  text-align: center;
  background: #de9f27;
  color: #000;
  font-size: 0.3rem;
  padding-top: 0.2rem;
  padding-bottom: 0.1rem;
}
.spanc {
  display: inline-block;
  width: 100%;
  padding-top: 0.2rem;
  padding-bottom: 0.2rem;
}
.butt {
  width: 2.2rem;
  margin: 0;
}
.icony {
  /* float: right; */
  position: absolute;
  right: 4px;
  top: 4px;
}
.llc {
  position: absolute;
  z-index: 11;
  top: 10%;
  left: 5%;
  display: block;
  width: 90%;
  height: 80%;
}
.img3 {
  height: 100%;
  width: 100%;
}
.div1 {
  min-height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
.lpan {
  display: inline-block;
  width: 3rem;
  line-height: 1rem;
}
.bug {
  display: block;
  width: 90%;
  height: 90%;
  /* margin-left: 5%; */
  /* margin-bottom: 1rem; */
  overflow-y: auto;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
