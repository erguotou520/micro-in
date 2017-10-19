<!-- 右侧icon相距20px -->
<template>
  <div class="header" :class="['bg-' + type]" :style="bg?{'background-color':bg}:{}">
    <div class="left" @click="$emit('left-click')">
      <text v-if="!$slots.left">&nbsp;</text>
      <slot name="left"></slot>
    </div>
    <text class="title"><slot></slot></text>
    <div class="right" @click="$emit('right-click')">
      <text v-if="!$slots.right">&nbsp;</text>
      <slot v-else name="right"></slot>
    </div>
  </div>
</template>
<script>
export default {
  name: 'u-header',
  props: {
    type: {
      type: String,
      default: 'primary'
    },
    bg: String
  }
}
</script>
<style lang="stylus" scoped>
@import "../../css/variable.styl"
.header
  flex-direction row
  justify-content space-between
  align-items center
  height 88px
  padding-left 20px
  padding-right 20px
.bg-primary
  background-color $color-primary
.left
  flex-direction row
  align-items center
  flex 1
  min-width 40px
  height 100%
.right
  flex-direction row
  justify-content flex-end
  align-items center
  flex 1
  height 100%
.title
  flex 4
  color #fff
  font-size 34px
  font-weight 300
  text-align center
</style>
