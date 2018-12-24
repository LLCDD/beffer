<template>
  <div class="div1" style="width:100%">
    <table style="hegiht:100%">
      <tr>
        <th>描述</th>
        <th>数量</th>
        <th>时间</th>
      </tr>
      <tr v-for="(item,index) in list" :key="index" v-if="ti">
        <td>{{ item.note.note }}</td>
        <td class="span1">{{ item.num }}</td>
        <td>{{ item.created_at }}</td>
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
      msg: "3454",
      list: [
        {
          id: 753,
          user_id: 63,
          type: 1,
          status: 1,
          before: "0.00",
          num: "",
          log_type: 20,
          created_at: "",
          updated_at: "",
          note: {
            id: 20,
            note: ""
          }
        }
      ],
      firstpage: "1",
      lastpage: "2",
      bool: false,
      page: 1,
      ti: false
    };
  },
  mounted() {
    this.$store.commit("fanhui", true);
    const _this = this;
    _this.http
      .post("/api/center/wallet_log", { type: 0 })
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
          _this.ti = true;
          _this.list = res.data.list.data;
        } else if (res.code == 400) {
          _this.$toasted.success(res.message, { icon: "error" }).goAway(1500);
        }
      })
      .catch(res => {
        _this.$roasted.success(res.message, { icon: "error" }).goAway(1000);
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
        .post("/api/center/wallet_log", { page: _this.firstpage, type: 0 })
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
        .post("/api/center/wallet_log", { page: _this.firstpage, type: 0 })
        .then(res => {
          _this.list = res.data.list.data;
        });
    }
  }
};
</script>
<style scoped>
.div1 {
  color: #fff;
  min-height: 10rem;
  overflow: hidden;
}
.span {
  display: inline-block;
  padding-left: 1.2rem;
  padding-right: 0.5rem;
  padding-top: 0.45rem;
  font-size: 0.3rem;
  color: #5c81c4;
  padding-bottom: 0.34rem;
}
.div2 {
  width: 100%;
  border-bottom: 1px solid #5c81c4;
}
.div3 {
  font-size: 0.26rem;
  border-bottom: 1px solid #5c81c4;
  float: left;
}
.div3 > span {
  display: inline-block;
  padding-left: 0.7rem;
  padding-top: 0.3rem;
  padding-bottom: 0.34rem;
}
.span1 {
  color: red;
  width: 19%;
}
table {
  width: 100%;
  border: 0;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
}
tr {
  width: 100%;
  text-align: center;
  border: 0;
}
th {
  font-size: 0.3rem;
  border: 0;
  color: #5c81c4;
  padding-top: 0.45rem;
  padding-bottom: 0.34rem;
  border-bottom: 1px solid #5c81c4;
  width: 33.3%;
}
td {
  border: 0;
  border-bottom: 1px solid #5c81c4;
  padding-top: 0.3rem;
  padding-bottom: 0.34rem;
}
.div2 {
  height: 1rem;
  padding-top: 0.3rem;
  text-align: center;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
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