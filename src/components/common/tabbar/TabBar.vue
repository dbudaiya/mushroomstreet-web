<script>
export default {
  name: 'TabBar',
  data() {
    return {
      active: 0,
    }
  },
}
</script>

<template>
  <van-tabbar
    v-model="active"
    :border="false"
    :z-index="100"
    active-color="#ff8198"
    replace
    route
    class="tab-bar"
  >
    <van-tabbar-item icon="wap-home" to="/">
      首页
    </van-tabbar-item>
    <van-tabbar-item icon="shop" to="/category">
      分类
    </van-tabbar-item>
    <van-tabbar-item icon="shopping-cart" to="/cart">
      购物车
    </van-tabbar-item>
    <van-tabbar-item icon="manager" to="/profile">
      我的
    </van-tabbar-item>
  </van-tabbar>
</template>

<style scoped>
/deep/ .van-tabbar {
  border-top: 1px solid #cccccc;
  background-color: #f6f6f6;
}
/deep/ .van-tabbar-item {
  font-size: 13px;
}

/deep/ .van-tabbar-item__icon {
  font-size: 22px;
}
</style>
