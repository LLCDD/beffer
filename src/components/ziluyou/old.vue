<template>
  <div class="div">
    <table>
      <tr>
        <th>订单状态</th>
        <th>数量</th>
        <th>价格</th>
        <th>时间</th>
      </tr>
      <tr v-for="(item,index) in list.list.data" :key="index">
        <td @click="ok(index,item.id)" ref="ipt">{{ item.status == 3 ? "已完成" : "已取消" }}</td>
        <td>{{ item.num }}</td>
        <td>{{ item.price }}</td>
        <td>{{ item.updated_at }}</td>
      </tr>
    </table>
    <div class="div2" v-if="bool">
      <button @click="previousPage()">上一页</button>
      <span>{{firstpage}}/{{lastpage}}</span>
      <button @click="nextPage()">下一页</button>
    </div>
    <div class="div2" v-if="!bool">没有更多数据了</div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "平台支付",
      money: "100.00",
      list: {
        list: {
          current_page: 1,
          data: [
            {
              id: 16,
              user_id: 63,
              price: "0.65",
              num: 100,
              status: 4,
              type: 2,
              created_at: "2018-12-17 16:46:13",
              no: "D2018121753515756",
              updated_at: "2018-12-17 17:47:34",
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
            }
          ]
        },
        login_id: 63
      },
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1
    };
  },
  mounted() {
    this.$store.commit("fanhui", true);
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    const _this = this;
    _this.http
      .post("/api/deal/history")
      .then(res => {
        if (res.code == 200) {
          console.log(res.data);
          _this.firstpage = res.data.list.current_page;
          _this.lastpage = res.data.list.last_page;
          if (res.data.list.last_page > 1) {
            _this.bool = true;
          } else {
            _this.bool = false;
          }
          _this.list = res.data;
        } else if (res.code == 400) {
          _this.$toasted.success(res.message, { icon: "error" }).goAway(1500);
        }
      })
      .catch(res => {
        _this.$roasted.success(res.message, { icon: "error" }).goAway(1000);
      });
  },
  methods: {
    ok(i, id) {
      // if (this.$refs.ipt[i].innerHTML == "已完成") {
      this.$router.push("/success/" + id);
      // } else {
      // this.$toasted.error("取消订单没有信息", { icon: "error" }).goAway(1200);
      // }
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
        .post("/api/deal/history", { page: _this.firstpage })
        .then(res => {
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
      _this.http
        .post("/api/deal/history", { page: _this.firstpage })
        .then(res => {
          _this.list = res.data.list.data;
        });
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
}
.span {
  padding-left: 1.2rem;
}
table {
  padding-left: 0.18rem;
  padding-right: 0.24rem;
  width: 100%;
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
</style>
