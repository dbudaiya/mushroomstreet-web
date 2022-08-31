<script>
export default {
  name: 'DetailSwiper',
  props: {
    swiperList: {
      type: Array,
      // default默认值需要一个函数return一个默认值
      default() {
        return []
      },
    },
  },
  data() {
    return {
      isLoad: true,
    }
  },
  methods: {
    swiperLoad() {
      if (this.isLoad) {
        this.$emit('swiperLoad')
        this.isLoad = false
      }
    },
  },
}
</script>

<template>
  <div class="swiper-list">
    <van-swipe :autoplay="4000" indicator-color="#ff0000">
      <van-swipe-item v-for="(item, index) in swiperList" :key="index">
        <img :src="item" alt="" @load="swiperLoad">
      </van-swipe-item>
    </van-swipe>
  </div>
</template>

<style scoped>
.swiper-list /deep/ .van-swipe__indicator {
  width: 10px;
  height: 10px;
  opacity: 0.8;
  border-radius: 100%;
  background-color: #ffffff;
}

.swiper-list {
  overflow: hidden;
  width: 100%;
  height: 300px;
}

.swiper-list img {
  display: block;
  width: 100%;
  height: 300px;
  object-fit: cover;
  object-position: center top;
}
</style>
