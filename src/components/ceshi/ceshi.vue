<template>
  <div>
    <ul>
      <li v-for="(item,index) in list" :key="index" ref="val" @click="top(index)">{{ index }}</li>
    </ul>
    <button @click="s()">上一页</button>
    <button @click="x()">下一页</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      list: [1, 2, 3, 4]
    };
  },
  methods: {
    top(i) {
      console.log(this.$refs.val[i].innerHTML);
    }
  },
  mounted() {
    if (this.list.length > 1) {
      console.log(this.list[this.list.length - 1]);
    }
  }
};
</script>
<style scoped>
ul {
  padding-top: 0.8rem;
  height: 100%;
  position: absolute;
}
li {
  float: left;
  width: 20px;
  margin-top: 1rem;
  border: 1px solid #ccc;
  text-align: center;
}
</style>
