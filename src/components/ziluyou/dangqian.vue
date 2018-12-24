<template>
  <div class="div" style="width:100% ; overflow: hidden;">
    <van-pull-refresh class="pull" v-model="isLoading" @refresh="onRefresh">
      <table class="table">
        <tr>
          <th>单号</th>
          <th>数量</th>
          <th>价格</th>
          <th>时间</th>
        </tr>
        <tr v-for="(item,index) in list" :key="index" @click="top(item.id)" v-if="no">
          <td>{{ item.no }}</td>
          <td>{{ item.num }}</td>
          <td>{{ item.price }}</td>
          <td>{{ item.created_at }}</td>
        </tr>
      </table>
      <div class="div2" v-if="bool" style="position: relative;">
        <button @click="previousPage()">上一页</button>
        <span>{{firstpage}}/{{lastpage}}</span>
        <button @click="nextPage()">下一页</button>
      </div>
      <div class="div2" v-if="!bool" style="position: relative;">没有更多数据了</div>
    </van-pull-refresh>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isLoading: false,
      msg: "平台支付",
      money: "100.00",
      list: [
        {
          Order: "",
          said: "",
          num: "",
          money: "",
          timer: ""
        }
      ],
      list1: {},
      list2: [],
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1,
      no: false
    };
  },
  mounted() {
    this.$store.commit("fanhui", false);
    // this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    const _this = this;
    _this.http
      .post("/api/deal/now")
      .then(res => {
        if (res.code == 200) {
          console.log(res.data);
          _this.firstpage = res.data.list.current_page;
          _this.lastpage = res.data.list.last_page;
          _this.no = true;
          if (res.data.list.last_page > 1) {
            _this.bool = true;
          } else {
            _this.bool = false;
          }
          _this.list1 = res.data;
          _this.list = res.data.list.data;
        } else if (res.code == 400) {
          _this.$toasted.success(res.message, { icon: "error" }).goAway(1500);
        }
      })
      .catch(res => {
        _this.$roasted.success(res.message, { icon: "error" }).goAway(1000);
      });
    // _this.http
    //   .post("/api/deal/now")
    //   .then(res => {
    //     if (res.code == 200) {
    //       _this.list = res.data.list.data;
    //     } else if (res.code == 400) {
    //       this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
    //     }
    //   })
    //   .catch(res => {
    //     _this.$toasted.error(res.message, { icon: "error" }).goAway(2000);
    //   });
  },
  methods: {
    top(a) {
      this.$router.push("/orderDetails/" + a);
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
      _this.http.post("/api/deal/now", { page: _this.firstpage }).then(res => {
        _this.list = res.data.list.data;
      });
    },
    nextPage() {
      var count = this.firstpage;
      count++;
      if (count >= this.lastpage) {
        count = this.lastpage;
      }
      this.firstpage = count;
      const _this = this;
      _this.http.post("/api/deal/now", { page: _this.firstpage }).then(res => {
        _this.list = res.data.list.data;
      });
    },
    onRefresh() {
      const _this = this;
      setTimeout(() => {
        _this.http
          .post("/api/deal/now")
          .then(res => {
            if (res.code == 200) {
              // console.log(res.data);
              _this.firstpage = res.data.list.current_page;
              _this.lastpage = res.data.list.last_page;
              this.$toast("刷新成功");
              this.isLoading = false;
              if (res.data.list.last_page > 1) {
                _this.bool = true;
              } else {
                _this.bool = false;
              }
              _this.list1 = res.data;
              _this.list = res.data.list.data;
            } else if (res.code == 400) {
              _this.$toasted
                .success(res.message, { icon: "error" })
                .goAway(1500);
            }
          })
          .catch(res => {
            _this.$roasted.success(res.message, { icon: "error" }).goAway(1000);
          });
      }, 500);
    }
  }
};
</script>
<style scoped>
.div1 > span {
  display: inline-block;
  padding-top: 0.56rem;
  color: #6086c7;
  width: 33.3%;
  overflow: hidden;
}
.span {
  padding-left: 1.2rem;
}
table {
  padding-left: 0.18rem;
  padding-right: 0.24rem;
  width: 100%;
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
}
tr {
  text-align: center;
  border: 0;
}
th {
  padding-top: 0.56rem;
  font-size: 0.3rem;
  padding-bottom: 0.32rem;
  color: #5e85d2;
  border: 0;
  border-bottom: 1px solid #5d8eeb;
  width: 24.2%;
}
td {
  color: #ffffff;
  padding: 0.32rem 0;
  border: 0;
  border-bottom: 1px solid #5d8eeb;
}
.td {
  color: red;
}
.div2 {
  height: 1rem;
  padding-top: 0.3rem;
  text-align: center;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
  margin-bottom: 1rem;
}
.div2 > button:first-child {
  float: left;
  margin-left: 0.3rem;
  background: green;
  color: #fff;
  border-radius: 0.1rem;
  width: 1rem;
}
.div2 > button:last-child {
  float: right;
  margin-right: 0.3rem;
  background: green;
  color: #fff;
  border-radius: 0.1rem;
  width: 1rem;
}
.pull {
  height: 100% !important;
  overflow: hidden;
  background: url("../../assets/image/500585755_banner.png") no-repeat !important;
  background-size: cover;
}
</style>
