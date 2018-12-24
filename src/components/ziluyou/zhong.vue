<template>
  <div class="div" style="padding-bottom: 1rem; width:100%;height:100%">
    <van-pull-refresh class="pull" style="height:100%" v-model="isLoading" @refresh="onRefresh">
      <table>
        <tr>
          <th>单号</th>
          <th>时间</th>
          <th>处理状态</th>
        </tr>
        <tr
          v-if="ok"
          v-for="(item,index) in list.list.data"
          :key="index"
          @click="detail(item.deal.id)"
        >
          <td>{{ item.deal.no }}</td>
          <td>{{ item.created_at }}</td>
          <td>
            <span v-if="ok">{{ item.status==0?"未处理":"已处理"}}</span>
          </td>
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
      isLoading: false,
      msg: "平台支付",
      money: "",
      list: {
        list: {
          current_page: 1,
          data: [
            {
              id: 4,
              user_id: 63,
              deal_id: 7,
              text: "",
              status: 0,
              created_at: "",
              updated_at: "",
              deal: {
                id: 7,
                no: ""
              }
            }
          ],
          first_page_url: "http://192.168.0.189/api/deal/complaint?page=1",
          from: 1,
          last_page: 1,
          last_page_url: "http://192.168.0.189/api/deal/complaint?page=1",
          next_page_url: null,
          path: "http://192.168.0.189/api/deal/complaint",
          per_page: 10,
          prev_page_url: null,
          to: 3,
          total: 3
        },
        login_id: 63
      },
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1,
      ok: false
    };
  },
  mounted() {
    this.$store.commit("fanhui", false);
    this.$store.commit("tishi1", true);
    this.$store.commit("tishi", true);
    this.$store.commit("tabBar", true);
    const _this = this;
    // _this.http.post("/api/deal/complaint").then(res => {
    //   if (res.code == 200) {
    //     console.log(res);
    //     _this.list = res.data;
    //     console.log(_this.list);
    //   }
    // });
    _this.http
      .post("/api/deal/complaint")
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
          _this.ok = true;
        } else if (res.code == 400) {
          _this.$toasted.success(res.message, { icon: "error" }).goAway(1500);
        }
      })
      .catch(res => {
        _this.$roasted.success(res.message, { icon: "error" }).goAway(1000);
      });
  },
  methods: {
    detail(id) {
      this.$router.push("/orderDetails/" + id);
      localStorage.setItem("zhong", id);
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
        .post("/api/deal/complaint", { page: _this.firstpage })
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
        .post("/api/deal/complaint", { page: _this.firstpage })
        .then(res => {
          _this.list = res.data.list.data;
        });
    },
    onRefresh() {
      const _this = this;
      setTimeout(() => {
        _this.http.post("/api/deal/complaint").then(res => {
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
            _this.ok = true;
            this.$toast("刷新成功");
            this.isLoading = false;
          }
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
}
.span {
  padding-left: 1.2rem;
}
table {
  padding-left: 0.18rem;
  padding-right: 0.24rem;
  width: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
  height: 100%;
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
  width: 33.3%;
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
  color: #fff;
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
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
}
</style>
