<template>
  <div class="tabbar-item" @click="onClick">
    <slot v-if="!actived" name="normal"></slot>
    <slot v-else name="active"></slot>
    <text v-if="!$slots.normal&&$parent&&$parent.imageType==='iconfont'"
      class="icon-font" :class="[actived ? 'icon-font-active' : '']" :style="{fontFamily}"><slot></slot></text>
    <image v-if="!$slots.normal&&$parent&&$parent.imageType==='image'"
      class="icon-image" :src="actived?normalUrl:activeUrl"></image>
    <text class="text" :class="[actived ? 'text-active' : '']">{{label}}</text>
  </div>
</template>
<script>
const modal = weex.requireModule('modal')
export default {
  name: 'u-tabbar-item',
  props: {
    label: {
      type: String,
      required: true
    },
    index: String,
    // 图片模式的地址
    normalUrl: String,
    activeUrl: String,
    // iconfont模式的字体family
    fontFamily: {
      type: String,
      default: 'iconfont'
    }
  },
  data () {
    return {
      $parent: null
    }
  },
  computed: {
    actived () {
      return this.$parent && this.$parent.value === this.index
    }
  },
  methods: {
    onClick () {
      if (this.$parent) {
        this.$parent.selectBar(this.index)
        this.$emit('click')
      }
    }
  },
  created () {
    let parent = this.$parent
    while (parent.$parent && parent.$options._componentTag !== 'u-tabbar') {
      parent = parent.$parent
    }
    if (parent.$options._componentTag !== 'u-tabbar') {
      modal.toast({ message: 'u-tabbar-item不能单独使用', duration: 0.3 })
      return
    }
    this.$parent = parent
  },
  beforeDestroy () {
    this.$parent && this.$parent.removeBar(this)
  }
}
</script>
<style lang="stylus" scoped>
@import "../../css/variable.styl"
.tabbar-item
  flex 1
  height 100px
  align-items center
  justify-content center
.icon-font
  color $color-text-minor
  font-size 40px
.icon-font-active
  color $color-primary
.icon-image
  max-width 40px
  max-height @max-width
.text
  color $color-text-minor
  font-size 20px
.text-active
  color $color-primary
</style>
