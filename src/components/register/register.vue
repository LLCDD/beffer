<template>
  <div class="all">
    <rs-header></rs-header>
    <rs-content></rs-content>
  </div>
</template>

<script>
import RsHeader from "./components/RsHeader";
import RsContent from "./components/RsContent";
export default {
  components: {
    RsHeader,
    RsContent
  },
  created: function() {
    this.$store.state.headerStatus = false;
  }
};
</script>

<style  scoped>
.all {
  height: 100%;
  background: url("../../assets/image/500585755_banner.png") no-repeat;
  background-size: cover;
}
</style>