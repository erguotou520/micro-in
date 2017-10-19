<template>
  <div class="tabbar">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'u-tabbar',
  props: {
    value: {
      type: String
    },
    imageType: {
      type: String,
      // 支持iconfont image
      default: 'iconfont'
    }
  },
  data () {
    return {
      bars: []
    }
  },
  computed: {
    activeIndex () {
      return this.bars.map(bar => bar.index).indexOf(this.value) || 0
    }
  },
  methods: {
    addBar (bar) {
      this.bars.push(bar)
    },
    removeBar (bar) {
      this.bars.splice(this.bars.indexOf(bar), 1)
    },
    selectBar (bar) {
      this.$emit('input', bar)
      this.$emit('change', bar)
    }
  }
}
</script>
<style lang="stylus" scoped>
@import "../../css/variable.styl"
.tabbar
  position relative
  flex-direction row
  align-items center
  background-color #fff
  border-top-style solid
  border-top-color $color-text-disabled
  border-top-width 2px
</style>
