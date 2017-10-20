<template>
  <div class="apps">
    <u-header>微应用</u-header>
    <list class="apps-container">
      <cell class="app-cell" @click="goto('vue-weekly')">
        <image class="app-icon" src="https://vuejs.org/images/logo.png"></image>
        <text class="app-desc">Vue FEweekly (由VueJs中文社区开发)</text>
      </cell>
      <cell class="app-cell" @click="goto('zhihu-daily')">
        <image class="app-icon" src="http://ec4.images-amazon.com/images/I/41BkNRXccUL._SL500_AA300_.png"></image>
        <text class="app-desc">知乎日报Lite</text>
      </cell>
    </list>
  </div>
</template>
<script>
import 'Config'
import Header from 'Components/navigation/Header'
export default {
  components: {
    [Header.name]: Header
  },
  methods: {
    goto (name) {
      this.$router.open({ name })
    }
  }
}
</script>
<style lang="stylus" scoped>
@import "../../css/variable.styl"
@import "../../css/function.styl"
// .apps-container
  // border-top(1px, solid, $color-text-disabled)
  // border-bottom(1px, solid, $color-text-disabled)
.app-cell
  display flex
  flex-direction row
  align-items center
  padding 20px
.app-icon
  width 88px
  height 88px
  border-radius 50%
.app-desc
  margin-left 20px
  color $color-text-primary
  font-size 32px
</style>
