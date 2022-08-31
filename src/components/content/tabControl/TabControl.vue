<script>
export default {
  name: 'TabControl',
  props: {
    titles: {
      type: Array,
      default() {
        return []
      },
    },
  },
  data() {
    return {
      curIndex: 0,
    }
  },
  methods: {
    tabControlClick(index) {
      if (this.curIndex === index)
        return
      this.curIndex = index
      this.$emit('tabClick', index)
    },
  },
}
</script>

<template>
  <div class="tab-control">
    <div
      v-for="(item, index) in titles"
      :key="index"
      :class="{ active: index === curIndex }"
      class="tab-control-item"
      @click="tabControlClick(index)"
    >
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<style scoped>
.tab-control {
  line-height: 40px;
  display: flex;
  justify-content: space-between;
  height: 40px;
  text-align: center;
}

.tab-control-item {
  flex: 1;
}

.tab-control-item span {
  font-size: 15px;
  padding: 5px;
}

.active {
  color: #ff5777;
}

.active span {
  border-bottom: 2px solid #ff5777;
}
</style>
