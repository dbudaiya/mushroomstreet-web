<script>
import RecommendView from './children/RecommendView'
import FeatureView from './children/FeatureView'
import NavBar from '@/components/common/navbar/NavBar'
import Scroll from '@/components/common/scroll/Scroll'
import TabControl from '@/components/content/tabControl/TabControl'
import GoodsList from '@/components/content/goods/GoodsList'
import BackTop from '@/components/content/backTop/BackTop'

import MySwiper from '@/components/common/swiper/MySwiper'

import { getHomeGoodsData, getHomeMultiData } from '@/network/home'
import { backTopMixin, imgListenerMixin } from '@/common/mixin'

export default {
  name: 'Home',
  components: {
    NavBar,
    Scroll,
    TabControl,
    GoodsList,
    BackTop,
    MySwiper,
    RecommendView,
    FeatureView,
  },
  // vue重复代码的混入
  mixins: [imgListenerMixin, backTopMixin],
  data() {
    return {
      banners: [],
      recommends: [],
      tabControlTitles: ['流行', '新款', '精选'],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      curType: 'pop',
      // 当前吸顶的位置
      tabOffsetTop: 0,
    }
  },
  computed: {
    isShowTab() {
      return this.curPosition >= this.tabOffsetTop
    },
  },
  created() {
    this.getHomeMultiData()
    this.getHomeGoodsData('pop')
    this.getHomeGoodsData('new')
    this.getHomeGoodsData('sell')
  },
  // keep-alive状态下进来的时候的生命周期
  activated() {
    // 同时刷新better-scroll防止不能滚动
    this.$refs.scroll.refresh()
  },
  // keep-alive状态下离开的时候的生命周期
  deactivated() {
    // 取消home组件事件总线的监听
    this.$bus.$off('imgLoad', this.imgListener)
  },
  methods: {
    // 1、请求多个数据
    getHomeMultiData() {
      getHomeMultiData().then((res) => {
        // console.log(res);
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
    // 2、请求商品数据
    getHomeGoodsData(type) {
      const page = this.goods[type].page + 1
      getHomeGoodsData(type, page).then((res) => {
        // console.log(res.data.list);
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        // 调用finishPullUp可以一直加载更多
        this.$refs.scroll.finishPullUp()
      })
    },
    // 获取滚动的位置
    backTopScroll(position) {
      this.curPosition = position ? -position.y : 0
    },
    // tabControl点击事件
    tabClick(index) {
      if (index === 0)
        this.curType = 'pop'
      else if (index === 1)
        this.curType = 'new'
      else if (index === 2)
        this.curType = 'sell'

      this.$refs.tabControl.curIndex = index
      this.$refs.tabControl2.curIndex = index
    },
    // 上拉加载
    loadMore() {
      this.getHomeGoodsData(this.curType)
    },
    // 监听轮播图加载完成
    swiperLoad() {
      // 获取tabControl的位置
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    },
  },
}
</script>

<template>
  <div class="home">
    <!-- 标题栏 -->
    <NavBar class="home-nav">
      <div slot="center">
        购物街
      </div>
    </NavBar>

    <TabControl
      v-show="isShowTab"
      ref="tabControl"
      :titles="tabControlTitles"
      class="home-tab-control"
      @tabClick="tabClick"
    />

    <!-- BScroll滚动区域 -->
    <Scroll
      ref="scroll"
      :probe-type="3"
      :pull-up-load="true"
      class="scroll-height"
      @backTopScroll="backTopScroll"
      @pullingUp="loadMore"
    >
      <!-- 轮播图 -->
      <MySwiper :swiper-list="banners" @swiperLoad="swiperLoad" />
      <!-- 推荐分类 -->
      <RecommendView :recommends="recommends" />
      <!-- 本周流行 -->
      <FeatureView />
      <!-- tabBarControl -->
      <TabControl
        ref="tabControl2"
        :titles="tabControlTitles"
        class="home-tab-control"
        @tabClick="tabClick"
      />
      <!-- 商品展示 -->
      <GoodsList :goods="goods[curType].list" />
    </Scroll>

    <!-- 回到顶部,监听组件的原生事件必须要用native修饰符变成原生组件 -->
    <transition name="scroll">
      <BackTop v-show="curPosition >= 1500" @click.native="backTop" />
    </transition>
  </div>
</template>

<style scoped>
.home {
  position: relative;
  height: 100%;
}

.home .home-nav {
  font-weight: 600;
  color: white;
  background-color: #ff8198;
}

/* 导航吸顶 */
.home-tab-control {
  position: relative;
  z-index: 3;
  background-color: white;
}

/* BScroll固定高度和区域 */
.scroll-height {
  position: absolute;
  top: 44px;
  right: 0;
  bottom: 50px;
  left: 0;
  overflow: hidden;
}

/* vue的淡入淡出动画 */
.scroll-enter-active,
.scroll-leave-active {
  transition: all 0.3s;
}

.scroll-enter,
.scroll-leave-to {
  opacity: 0;
}

.scroll-enter-to,
.scroll-leave {
  opacity: 1;
}
</style>
