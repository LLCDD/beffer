<template>
  <div class="div1">
    <div class="div2" :class="{buton:bool}">
      <span class="span1">卖入估价：</span>
      <span>{{ msg }}</span>
    </div>
    <div class="div2" :class="{buton:bool}">
      <span class="span1">卖入数量：</span>
      <input class="input" v-model="input" type="text" placeholder="请输入数量">
      <!-- <span>HGH：0.00</span> -->
    </div>
    <div class="div2" :class="{buton:bool}">
      <span class="span1">兑换基数：</span>
      <span>1000</span>
    </div>
    <div class="div2" :class="{buton:bool}">
      <span class="span1">卖出金额：</span>
      <span>{{ (msg * input).toFixed(2) }}</span>
    </div>
    <button @click="top()" :class="{buton:bool}">发布</button>
    <div class="div3 buton">
      <span class="span2">
        市场挂单
        <span class="span3">（只显示买家在线）</span>
      </span>
    </div>
    <div :class="{buton:bool}" class="buy">
      <div v-for="(item,index) in list.list.data" :key="index">
        <div class="div5">
          <div class="div4">
            <p class="p1">{{ item.name }}</p>
            <p>时间：{{ item.created_at }}</p>
            <p>兑换基数：{{ item.num }}</p>
            <i class="iconfont" style="color:#de8e27">&#xe68f;</i>
          </div>
          <div class="div4 div6">
            <p class="p1">HGH：{{ item.HGH }}</p>
            <p>价格：{{ item.price }}</p>
            <p>数量：{{ item.num }}</p>
            <P>限额：{{ item.xian }}</P>
          </div>

          <div class="div8" @click="xq(item.id)">
            <div class="div4 div6 div7">卖出</div>
            <i class="iconfont1 icon3">&#xe604;</i>
          </div>
        </div>
      </div>
    </div>
    <div class="cdiv" v-if="this.bool">
      <p>是否卖出</p>
      <div class="cdiv1" @click="quxioa()">取消</div>
      <div class="cdiv1 cdiv2" @click="ok()">确定</div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // 买入的估价
      msg: "0.00",
      cid: "",
      bool: false,
      input: "",
      list: {
        list: {
          data: [
            {
              num: "1",
              created_at: "1",
              id: "1",
              type: "1",
              puser: { avatar: "1", id: 1, price: 0.65 }
            }
          ]
        }
      }
    };
  },
  methods: {
    top() {
      const _this = this;
      _this.http
        .post("/api/deal/sell_make", { num: _this.input })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success("挂单成功").goAway(1500);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1200);
        });
    },
    xq(a) {
      this.cid = a;
      console.log(a);
      this.bool = true;
    },
    quxioa() {
      this.bool = false;
    },
    ok() {
      this.bool = false;
      const _this = this;
      _this.http
        .post("/api/deal/match", { id: _this.cid })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success("匹配成功").goAway(1200);
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });

      _this.http.post("/api/deal/sell").then(res => {
        if (res.code == 200) {
          _this.msg = res.data.price;
          _this.list = res.data;
        }
      });
    }
  },
  mounted() {
    const _this = this;
    _this.http
      .post("/api/deal/sell")
      .then(res => {
        if (res.code == 200) {
          _this.msg = res.data.price;
          _this.list = res.data;
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(1000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(1000);
      });
  }
};
</script>
<style scoped>
@font-face {
  font-family: "iconfont";
  src: url("../../assets/font/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 0.22rem;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.div1 {
  margin-top: 0.22rem;
  width: 100%;
}
.div1 > .div2 {
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
  margin-top: 0.56rem;
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
.div3 {
  height: 1.3rem;
  line-height: 1.3rem;
  padding-left: 0.4rem;
}
.div3 > span {
  color: #68aefe !important;
}
.span2 {
  font-size: 0.34rem;
  color: #68aefe;
  opacity: 1 !important;
}
.span3 {
  font-size: 0.26rem;
}
.div5 {
  width: 100%;
  float: left;
  padding: 0 0.4rem;
  padding-bottom: 1rem;
}
.div4 {
  float: left;
}
.div4 > p {
  color: #68aefe;
  font-size: 0.24rem;
  margin-bottom: 0.16rem;
}
.p1 {
  font-size: 0.3rem !important;
  color: #fff !important;
}
.div6 {
  padding-left: 0.52rem;
}
.div7 {
  height: 0.84rem;
  width: 0.84rem;
  background: #3678e7;
  border-radius: 50%;
  text-align: center;
  line-height: 0.84rem;
  padding-left: 0;
  margin-top: 0.38rem;
  margin-left: 0.28rem;
}
@font-face {
  font-family: "iconfont1";
  src: url("../../assets/font1/iconfont.woff") format("woff");
}
.iconfont1 {
  font-family: "iconfont1" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.div8 {
  position: relative;
}
.icon3 {
  position: absolute;
  top: 0.64rem;
  right: -0.16rem;
  color: #f4f4f6;
  font-weight: 100;
}
.cdiv {
  height: 3rem;
  width: 80%;
  background: #ffffff;
  position: absolute;
  z-index: 12;
  top: 40%;
  left: 10%;
  text-align: center;
  border-top: 3px solid #3678e7;
}
.cdiv > p {
  padding-top: 0.6rem;
  font-size: 0.5rem;
  color: #3678e7;
}
.cdiv1 {
  position: absolute;
  bottom: 0;
  width: 49.7%;
  float: left;
  height: 0.7rem;
  line-height: 0.7rem;
  background: #fff;
  color: #ccc;
  border: 1px solid #ccc;
  border-bottom: 0;
}
.cdiv2 {
  left: 50%;
  color: #3678e7;
}
.buton {
  opacity: 0.2;
}
</style>