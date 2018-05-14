<template>
  <div class="left-nav">
      <ul>
          <li>
              <i class="icon iconfont icon-all"></i>
              <div>收银</div>
          </li>
          <li>
              <i class="icon iconfont icon-cart"></i>
              <div>店铺</div>
          </li>
          <li>
              <i class="icon iconfont icon-search"></i>
              <div>商品</div>
          </li>
          <li>
              <i class="icon iconfont icon-add"></i>
              <div>会员</div>
          </li>
          <li>
              <i class="icon iconfont icon-close"></i>
              <div>统计</div>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
  name: "leftNav",
  data() {
    return {};
  }
};
</script>

<style scoped>
.left-nav {
  color: #fff;
  font-size: 10px;
  height: 100%;
  background: #1d8ce0;
  float: left;
  width: 5%;
}
.iconfont {
  font-size: 30px;
}
.left-nav ul {
  margin: 0;
  padding: 0;
}
.left-nav li {
  list-style: none;
  padding: 10px;
  cursor: pointer;
  font-size: 16px;
}
.left-nav li div {
  padding: 4px 0;
}
</style>
