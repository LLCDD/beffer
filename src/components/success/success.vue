<template>
  <div class="div">
    <span class="llc" v-if="bool1">
      <img class="img3" :src="src" @click="tu()">
    </span>

    <p>
      <span>订单单号：</span>
      {{ info.deal.no }}
    </p>
    <p>
      <span>价格：</span>
      {{ info.deal.price }}
    </p>
    <p>
      <span>数量：</span>
      {{ info.deal.num }}
    </p>
    <p>
      <span>更新时间：</span>
      {{ info.deal.confirm_at }}
    </p>
    <p>
      <span>订单状态：</span>
      {{ state }}
    </p>
    <p>
      <span>订单总价：</span>
      {{ info.deal.price * info.deal.num }}
    </p>
    <p>
      <span>买卖状态：</span>
      {{ status }}
    </p>
    <button @click="other()" v-if="bd">查看对方信息</button>
    <button class="btn1" @click="other1()" v-if="bd">查看打款截图</button>
    <div v-if="bool" class="div2" :class="{wei:bool1}">
      <i class="iconfont icony" @click="icon()">&#xe615;</i>
      <div class="divl">
        <div class="div1">用户名：{{ obj.other.nickname }}</div>
        <div class="div3">
          <img :src="obj.other.avatar" alt>
        </div>
      </div>
      <div class="div4">
        <p class="p1 p2">对方支付宝信息</p>
        <span class="spanc">支付宝用户名：{{ obj.other.info.alipay_name }}</span>
        <br>
        <span class="spanc">支付宝账号:{{ obj.other.info.alipay_phone }}</span>
        <span class="spanc">
          支付宝收款码：
          <button class="butt" @click="screenshots()">点击查看收款码</button>

          <!-- <img class="img2" :src="obj.other.avatar" alt> -->
        </span>
      </div>
      <div class="div4">
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
      <div class="div4">
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
      <div class="div4 div5">
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
      msg: "登陆",
      info: {
        deal: {
          id: 1,
          user_id: 63,
          price: "1.00",
          num: 100,
          status: 3,
          type: 1,
          created_at: "2018-12-08 10:12:36",
          no: "2018091809535756",
          updated_at: "2018-12-19 14:50:55",
          deal_id: 99,
          img:
            "http://192.168.0.189/uploads/deal_img/20181218/5c184e300713a.jpeg",
          fee: 5,
          fee1: 5,
          pay_at: "2018-12-18 09:32:32",
          confirm_at: "2018-12-19 14:50:55",
          deleted_at: null,
          match_at: null,
          puser: {
            id: 63,
            nickname: "初始账号1",
            avatar:
              "http://192.168.0.189/uploads/head_img/20181217/5c16f845de4d6.jpeg"
          },
          auser: {
            id: 99,
            nickname: "pqf11111",
            avatar: "http://192.168.0.189/uploads/head_img/3.png"
          }
        },
        login_id: 63
      },
      obj: {
        other: {
          id: 99,
          phone: "18317894401",
          nickname: "pqf111",
          pid: 63,
          status: 0,
          avatar: "http://192.168.0.189/uploads/head_img/3.png",
          code: "666666",
          created_at: "2018-09-17 16:04:07",
          updated_at: "2018-12-17 16:25:52",
          token: "2fb887e0-fd26-11e8-be0d-3d423312f787",
          gender: 0,
          hgh_address: "6958C20945E12DBE7FFD82EE9BB12E7D",
          hgh_img:
            "http://192.168.0.189/uploads/hgh_img/6958c20945e12dbe7ffd82ee9bb12e7d.png",
          info: {
            id: 76,
            user_id: 99,
            created_at: "2018-09-17 16:29:35",
            updated_at: "2018-09-19 16:00:27",
            is_perfect: 2,
            bank_user: null,
            bank_type: null,
            bank_address: null,
            bank_num: null,
            alipay_name: null,
            alipay_phone: null,
            alipay_img: null,
            wechat_name: null,
            wechat_phone: null,
            wechat_img: null,
            imtoken: null,
            imtoken_img: null
          }
        }
      },
      bool: false,
      src: "",
      // 图片的出现
      bool1: false,
      bd: false
    };
  },
  computed: {
    state() {
      if (this.info.deal.status == "3") {
        return "订单已完成";
      } else {
        return "订单已取消";
      }
    },
    status() {
      if (this.info.deal.type == 1) {
        if (this.info.deal.user_id == this.info.login_id) {
          return "买入";
        } else {
          return "卖出";
        }
      } else if (this.info.deal.type == 2) {
        if (this.info.deal.user_id == this.info.login_id) {
          return "卖出";
        } else {
          return "买入";
        }
      }
    }
  },
  mounted() {
    this.$store.commit("size", "订单详情");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    this.$store.commit("fanhui", true);
    const _this = this;
    _this.http
      .post("/api/deal/detail", { id: _this.$route.params.id })
      .then(res => {
        if (res.code == 200) {
          _this.info = res.data;
          if (res.data.deal.status == 3) {
            _this.bd = true;
          }
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
      });
  },
  methods: {
    other() {
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
        this.bool1 = true;
      }
    },
    // 微信的支付截图
    screenshots1() {
      console.log("微信");
      if (this.obj.other.info.wechat_img == null) {
        this.$toasted.error("还没有截图", { icon: "error" }).goAway(1200);
      } else {
        this.src = this.obj.other.info.wechat_img;
        this.bool1 = true;
      }
    },
    // imToken 的支付截图
    screenshots2() {
      console.log("intoken");
      if (this.obj.other.info.imtoken_img == null) {
        this.$toasted.error("还没有截图", { icon: "error" }).goAway(1200);
      } else {
        this.src = this.obj.other.info.imtoken_img;
        this.bool1 = true;
      }
    },
    // 图片的消失
    tu() {
      this.bool1 = false;
    },
    other1() {
      this.bool1 = true;
      this.src = this.info.deal.img;
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
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  min-height: 100%;
  background-size: cover;
  color: #fff;
  overflow-y: auto;
  position: fixed;
  width: 100%;
}
p {
  padding-left: 0.4rem;
  padding-top: 0.4rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid #5d8eeb;
}
span {
  display: inline-block;
  width: 3rem;
}
button {
  background: #de9f27;
  color: #fff;
  border-radius: 4px;
  width: 2rem;
  height: 0.5rem;
  margin-top: 0.3rem;
  margin-left: 0.3rem;
}
.btn1 {
  float: right;
  margin-right: 0.3rem;
}
.div1 {
  padding-top: 0.4rem;
  width: 3rem;
  float: left;
}
.div2 {
  width: 80%;
  background: #323433;
  background-size: cover;
  margin-left: 10%;
  float: left;
  margin-top: 0.6rem;
  padding-top: 0.3rem;
  padding-left: 0.3rem;
  padding-right: 0.6rem;
  margin-bottom: 1rem;
  position: absolute;
  top: 5%;
  border-radius: 0.1rem;
}
.div3 {
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  overflow: hidden;
  float: right;
}
.div3 > img {
  display: block;
  height: 100%;
  width: 100%;
}
.divl {
  float: left;
  width: 100%;
  background: #de9f27;
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
.div4 {
  background: #323433;
  font-size: 0.3rem;
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
.div5 {
  padding-bottom: 0.3rem;
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
.dimg {
  display: block;
  height: 100%;
  width: 100%;
}
.spany {
  display: inline-block;
  width: 80%;
  margin-left: 10%;
  padding-bottom: 1rem;
}
.spanlly {
  float: right;
}
.icony {
  /* float: right; */
  position: absolute;
  right: 4px;
  top: 4px;
}
.wei {
  opacity: 0.3;
}
.p2 {
  margin-top: 1rem;
}
</style>
