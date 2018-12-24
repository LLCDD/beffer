<template>
  <div class="div">
    <van-pull-refresh style="height:100%" v-model="isLoading" @refresh="onRefresh">
      <table>
        <tr>
          <th>描述</th>
          <th>数量</th>
          <th>时间</th>
        </tr>
        <tr v-for="(item,index) in list.list.data" :key="index">
          <td>{{ item.note }}</td>
          <td class="td">{{ item.num }}</td>
          <td>{{ item.created_at }}</td>
        </tr>
      </table>
      <div class="div2" v-if="bool">
        <button @click="previousPage()">上一页</button>
        <span>{{firstpage}}/{{lastpage}}</span>
        <button @click="nextPage()">下一页</button>
      </div>
      <div class="div2" v-if="!bool">没有更多数据了</div>
    </van-pull-refresh>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "平台支付",
      money: "100.00",
      list: [
        {
          mian: "轩, 裂变子链转HGH...",
          num: "100.00",
          tiemr: "2018-11-20 03:45:12"
        },
        {
          mian: "轩, 裂变子链转HGH...",
          num: "100.00",
          tiemr: "2018-11-20 03:45:12"
        }
      ],
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1,
      isLoading: false
    };
  },
  mounted() {
    this.$store.commit("fanhui", true);
    this.$store.commit("size", "平台收支");
    this.$store.commit("tishi1", false);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", false);
    const _this = this;
    _this.http.post("/api/center/plat_log").then(res => {
      if (res.code == 200) {
        console.log(res);
        _this.list = res.data;
        if (res.data.list.last_page > 1) {
          _this.bool = true;
        } else {
          _this.bool = false;
        }
        console.log(_this.list);
      }
    });
  },
  methods: {
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
        .post("/api/center/plat_log", { page: _this.firstpage })
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
        .post("/api/center/plat_log", { page: _this.firstpage })
        .then(res => {
          _this.list = res.data.list.data;
        });
    },
    onRefresh() {
      setTimeout(() => {
        this.$toast("刷新成功");
        this.isLoading = false;
        _this.http.post("/api/center/plat_log").then(res => {
          if (res.code == 200) {
            console.log(res);
            _this.list = res.data;

            if (res.data.list.last_page > 1) {
              _this.bool = true;
            } else {
              _this.bool = false;
            }
          }
        });
      }, 500);
    }
  }
};
</script>
<style scoped>
.div {
  padding-top: 0.88rem;
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat bottom;
  background-size: cover;
}
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
  color: rgb(230, 196, 196);
  border-radius: 0.1rem;
  width: 1rem;
}
</style>
