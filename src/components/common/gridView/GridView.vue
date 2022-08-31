<script>
export default {
  name: 'GridView',
  props: {
    cols: {
      type: Number,
      default: 2,
    },
    hMargin: {
      type: Number,
      default: 8,
    },
    vMargin: {
      type: Number,
      default: 8,
    },
    itemSpace: {
      type: Number,
      default: 8,
    },
    lineSpace: {
      type: Number,
      default: 8,
    },
  },
  mounted() {
    setTimeout(this._autoLayout, 20)
  },
  updated() {
    this._autoLayout()
  },
  methods: {
    _autoLayout() {
      // 1.获取gridEl和children
      const gridEl = this.$refs.gridView
      const children = gridEl.children

      // 2.设置gridEl的内边距
      gridEl.style.padding = `${this.vMargin}px ${this.hMargin}px`

      // 3.计算item的宽度
      const itemWidth
        = (gridEl.clientWidth - 2 * this.hMargin - (this.cols - 1) * this.itemSpace) / this.cols
      for (let i = 0; i < children.length; i++) {
        const item = children[i]
        item.style.width = `${itemWidth}px`
        if ((i + 1) % this.cols !== 0)
          item.style.marginRight = `${this.itemSpace}px`

        if (i >= this.cols)
          item.style.marginTop = `${this.lineSpace}px`
      }
    },
  },
}
</script>

<template>
  <div ref="gridView" class="grid-view">
    <slot />
  </div>
</template>

<style scoped>
.grid-view {
  display: flex;
  flex-wrap: wrap;
}
</style>
