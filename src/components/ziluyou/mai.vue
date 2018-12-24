<template>
  <div class="div1">
    <p>
      <span class="span1">买入估价：</span>
      <span>{{ msg }}</span>
    </p>
    <p>
      <span class="span1">买入数量：</span>
      <input v-model="num" class="input" type="text" placeholder="请输入数量">
      <span>HGH</span>
    </p>
    <p>
      <span class="span1">兑换基数：</span>
      <span>1000</span>
    </p>
    <p>
      <span class="span1">买入金额：</span>
      <span>{{ sum }}</span>
    </p>
    <button @click="top()">发布</button>
    <!-- 新增 -->
    <div class="by" style="overflow: hidden;margin-bottom: 1rem;">
      <div v-for="(item,index) in list.list.data" :key="index" v-if="ti">
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
            <!-- <P>限额：{{ item.xian }}</P> -->
          </div>
          <div class="div8" @click="xq(item.id)">
            <div class="div4 div6 div7">买入</div>
            <i class="iconfont1 icon3">&#xe604;</i>
          </div>
        </div>
      </div>
      <!-- 是否买入 -->
      <div class="cdiv" v-if="this.bool2">
        <p>是否买入</p>
        <div class="cdiv1" @click="quxioa()">取消</div>
        <div class="cdiv1 cdiv2" @click="ok()">确定</div>
      </div>
      <!--  -->
      <div class="div10 div11" v-if="!bool">没有更多数据了</div>
      <div class="div10" v-if="bool">
        <button @click="previousPage()">上一页</button>
        <span>{{firstpage}}/{{lastpage}}</span>
        <button @click="nextPage()">下一页</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // 买入的估价
      msg: "0.00",
      num: "",
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
      },
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1,
      bool2: false,
      ti: false,
      count: 1
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
      this.count++;
      if (this.count < 2) {
        _this.http
          .post("/api/deal/buy_make", { num: _this.num })
          .then(res => {
            if (res.code == 200) {
              _this.$toasted.success(res.message).goAway(1500);
              _this.num = "";
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
          });
      } else {
        _this.num = "";
        _this.$toasted
          .error("您的操作过于频繁", { icon: "error" })
          .goAway(1500);
        setTimeout(function() {
          _this.count = 0;
        }, 3000);
      }
    },
    quxioa() {
      this.bool2 = false;
    },
    ok() {
      const _this = this;
      _this.http
        .post("/api/deal/match", { id: _this.cid })
        .then(res => {
          if (res.code == 200) {
            _this.$toasted.success("匹配成功").goAway(1200);
            _this.bool2 = false;
          } else if (res.code == 400) {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
          }
        })
        .catch(res => {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        });

      _this.http.post("/api/deal/index").then(res => {
        console.log(res);
        _this.list = res.data;
      });
    },
    previousPage() {
      console.log(this.firstpage);
      var count = this.firstpage;
      count--;
      this.firstpage = count;
      if (this.firstpage <= 1) {
        this.firstpage = 1;
      }
      const _this = this;
      _this.http
        .post("/api/deal/index", { page: _this.firstpage })
        .then(res => {
          if (res.code == 200) {
            _this.list = res.data;
          }
        });
    },
    xq(a) {
      this.cid = a;
      console.log(a);
      this.bool2 = true;
    },
    nextPage() {
      var count = this.firstpage;
      count++;
      if (count >= this.lastpage) {
        count = this.lastpage;
      }
      this.firstpage = count;
      const _this = this;

      _this.http
        .post("/api/deal/index", { page: _this.firstpage })
        .then(res => {
          _this.list = res.data;
        });
    }
  },
  mounted() {
    this.$store.commit("tabBar", true);
    this.$store.commit("tishi1", true);
    this.$store.commit("fanhui", false);
    const _this = this;
    _this.http
      .post("/api/deal/index")
      .then(res => {
        if (res.code == 200) {
          _this.msg = res.data.price;
          _this.list = res.data;
          _this.firstpage = res.data.list.current_page;
          _this.lastpage = res.data.list.last_page;
          _this.ti = true;
          if (res.data.list.last_page <= 1) {
            _this.bool = false;
          } else {
            _this.bool = true;
          }
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
.div1 {
  padding-top: 0.22rem;
  width: 100%;
}
.div1 > p {
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
.div10 {
  height: 1rem;
  padding: 0 !important;
  margin: 0 !important;
  padding-top: 0.3rem;
  text-align: center;
  border: 0 !important;
  float: left;
  width: 100%;
}
.div10 > button:first-child {
  float: left;
  margin-left: 0.3rem;
  background: green;
  color: #fff;
  border-radius: 0.1rem;
  width: 1rem;
}
.div10 > button:last-child {
  float: right;
  margin-right: 0.3rem;
  background: green;
  color: #fff;
  border-radius: 0.1rem;
  width: 1rem;
}
.div5 {
  width: 100%;
  float: left;
  padding: 0 0.4rem;
  padding-bottom: 0.3rem;
  margin-top: 0.4rem;
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
  float: right;
  margin-right: 0.4rem;
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
  position: fixed;
  z-index: 12;
  top: 45%;
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
.div11 {
  line-height: 1rem;
}
.by {
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
}
</style>






