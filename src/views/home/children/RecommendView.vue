<script>
export default {
  name: 'RecommendView',
  props: {
    recommends: {
      type: Array,
      default() {
        return []
      },
    },
  },
}
</script>

<template>
  <div class="recommend">
    <div v-for="(item, index) in recommends" :key="index" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <p>{{ item.title }}</p>
      </a>
    </div>
  </div>
</template>

<style scoped>
.recommend {
  display: flex;
  padding: 15px 0;
  border-bottom: 10px solid #eeeeee;
}

.recommend-item {
  flex: 1;
}

.recommend-item a {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}

.recommend-item img {
  width: 70px;
  height: 70px;
}

.recommend-item p {
  font-size: 14px;
  margin-top: 10px;
}
</style>
