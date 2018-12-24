<template>
  <div class="div1">
    <van-pull-refresh style="height:100%" v-model="isLoading" @refresh="onRefresh">
      <div class="div2">
        <span>
          今日大盘价 :
          <span class="span1">{{ info.price.base }}</span>
        </span>
      </div>
      <div class="div3">
        <p>
          {{ info.wallet.static_parent }}
          <br>
          <span>锁仓母链</span>
        </p>
        <p>
          {{ info.wallet.static_son }}
          <br>
          <span>释放子链</span>
        </p>
        <p>
          {{ info.wallet.dynamic_parent }}
          <br>
          <span>动态母链</span>
        </p>

        <router-link tag="p" to="/dynamic">
          {{ info.wallet.dynamic_son }}
          <br>
          <span>动态子链</span>
        </router-link>
      </div>
      <div class="div4">
        <div @click="liebian()">
          <i class="iconfont icon1" style="color:#f9a52e">&#xe62b;</i>
          裂变子链：
          <span>{{ info.wallet.release_son }}</span>
          <span class="span3">
            Fission chain
            <i class="iconfont icon2" style="color:#2c59a5">&#xe604;</i>
          </span>
        </div>
        <div @click="liebian1()">
          <i class="iconfont icon1" style="color:#f9a52e">&#xe62b;</i>
          锁仓母链：
          <span>{{ info.wallet.static_parent }}</span>
          <span class="span3">
            Lock parent
            <i class="iconfont icon2" style="color:#2c59a5">&#xe604;</i>
          </span>
        </div>
        <div @click="liebian2()">
          <i class="iconfont1 icon1" style="color:#f9a52e">&#xe64f;</i>
          壹区业绩：
          <span>{{ info.one }}</span>
          <span class="span3">
            One area
            <i class="iconfont icon2" style="color:#2c59a5">&#xe604;</i>
          </span>
        </div>
        <div @click="liebian3()">
          <i class="iconfont1 icon1" style="color:#f9a52e">&#xe64f;</i>
          贰区业绩：
          <span>{{ info.two }}</span>
          <span class="span3">
            Two area
            <i class="iconfont icon2" style="color:#2c59a5">&#xe604;</i>
          </span>
        </div>
        <div>
          <i class="iconfont1 icon1" style="color:#f9a52e">&#xe64f;</i>
          预估收益：
          <span>{{ sun }}</span>
          <span class="span3">
            Estimate
            <i class="iconfont icon2" style="color:#2c59a5">&#xe604;</i>
          </span>
        </div>
      </div>
    </van-pull-refresh>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      zi: "0.22",
      // 裂变子链
      msg1: "0.00",
      msg2: "0.00",
      isLoading: false,
      info: {
        wallet: {
          id: 16,
          user_id: 63,
          static_parent: "0.00",
          static_son: "0.00",
          release_son: "0.00",
          dynamic_parent: "0.00",
          dynamic_son: "0.00",
          hgh: "0.00",
          expect: "0.00",
          released: "0.00",
          created_at: "2018-12-08 10:42:18",
          updated_at: "2018-12-14 15:09:50",
          static_total: "0.00",
          dynamic_total: "0.00"
        },
        price: {
          base: 0.65,
          inc: 0.05
        },
        one: "0.00",
        two: "0.00",
        out: "0.00"
      }
    };
  },
  computed: {
    sun() {
      return this.info.wallet.static_total * this.info.out;
    }
  },
  mounted() {
    const _this = this;
    _this.http
      .post("/api/asset")
      .then(res => {
        if (res.code == 200) {
          // console.log(res.data);
          _this.info = res.data;
          // console.log(_this.info);
        } else if (res.code == 400) {
          _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
        }
      })
      .catch(res => {
        _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
      });

    // ----------------
    this.$store.commit("size", "我的资产");
    this.$store.commit("headerStatus", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    this.$store.commit("fanhui", false);
    // 发起我的资产的请求;

    // 今日的大盘价格
  },
  methods: {
    liebian() {
      this.$router.push("/zhuan/fission");
    },
    liebian1() {
      this.$router.push("/zizhuan/child");
    },
    liebian2() {},
    liebian3() {},
    onRefresh() {
      const _this = this;
      setTimeout(() => {
        _this.http
          .post("/api/asset")
          .then(res => {
            if (res.code == 200) {
              // console.log(res.data);
              _this.info = res.data;
              this.$toast("刷新成功");
              this.isLoading = false;
              // console.log(_this.info);
            } else if (res.code == 400) {
              _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
            }
          })
          .catch(res => {
            _this.$toasted.error(res.message, { icon: "error" }).goAway(1500);
          });
      }, 500);
    }
  }
};
</script>
<style scoped>
.div1 {
  padding-top: 0.88rem;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  min-height: 100%;
  overflow-y: auto;
  padding-bottom: 2rem;
}
.div2 {
  color: #ffffff;
  font-size: 0.28rem;
  height: 1.32rem;
  line-height: 1.32rem;
  padding-left: 0.64rem;
  margin-bottom: 0.34rem;
}
.span {
  padding-left: 0.9rem;
}
.span1 {
  color: #3feee8;
}
.div3 {
  height: 1.78rem;
  width: 90%;
  margin: 0 auto;
  background: #2b6bd9;
  border-radius: 0.1rem;
  margin-bottom: 0.28rem;
}
.div3 > p {
  color: #fff;
  display: inline-block;
  width: 24.8%;
  text-align: center;
  padding-top: 0.5rem;
  font-size: 0.34rem;
}
.div3 > p > span {
  display: inline-block;
  color: #bbcff3;
  font-size: 0.28rem;
  padding-top: 0.3rem;
}
.div4 {
  color: #fff;
  padding: 0 0.4rem;
}
.div4 > div {
  height: 1.28rem;
  width: 100%;
  line-height: 1.28rem;
  float: left;
}
.div4 > div > span {
  color: #32b7bb;
}
.icon1 {
  margin-right: 0.16rem;
}
.span3 {
  color: #ffffff !important;
  float: right;
}
@font-face {
  font-family: "iconfont";
  src: url("../../assets/font3/iconfont.woff") format("woff");
}
.iconfont {
  font-family: "iconfont" !important;
  font-size: 0.4rem;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
@font-face {
  font-family: "iconfont1";
  src: url("../../assets/font2/iconfont.woff") format("woff");
}
.iconfont1 {
  font-family: "iconfont1" !important;
  font-size: 0.4rem;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.icon2 {
  font-size: 0.3rem;
}
</style>