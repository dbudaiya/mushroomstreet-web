<script>
import { mapGetters } from 'vuex'
import CartList from './children/CartList'
import NavBar from '@/components/common/navbar/NavBar'

export default {
  name: 'Cart',
  computed: {
    // 对象写法可以自己命名
    ...mapGetters({
      cartLength: 'cartListLength',
    }),
  },
  components: { CartList, NavBar },
}
</script>

<template>
  <div class="cart">
    <!-- 导航 -->
    <NavBar class="nav-bar">
      <div slot="center">
        购物车(
        <span>{{ cartLength }}</span>
        )
      </div>
    </NavBar>
    <!-- 购物车列表 -->
    <CartList />
  </div>
</template>

<style scoped>
.nav-bar {
  font-weight: 600;
  color: white;
  background-color: #ff8198;
}
</style>
