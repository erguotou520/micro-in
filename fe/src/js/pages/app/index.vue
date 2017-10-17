<template>
  <div class="app">
    <!-- <embed :src="weexSrc" type="weex" class="content"></embed> -->
    <text class="content">123</text>
    <u-tabbar v-model="index" :style="{height:appHeight}" ref="tabbar">
      <u-tabbar-item index="apps">&#xe601;</u-tabbar-item>
      <u-tabbar-item index="tools">&#xe65a;</u-tabbar-item>
      <u-tabbar-item index="search">&#xe75e;</u-tabbar-item>
      <u-tabbar-item index="my">&#xe602;</u-tabbar-item>
    </u-tabbar>
  </div>
</template>
<script>
import 'Config'
import Tabbar from 'Components/navigation/Tabbar'
import TabbarItem from 'Components/navigation/TabbarItem'
const navs = ['apps', 'tools', 'search', 'my']
export default {
  globalEvent: {
    appActive() {
      console.log('active')
    },
    appDeactive() {
      console.log('deactive')
    }
  },
  data() {
    return {
      appHeight: `${weex.config.deviceHeight}px`,
      index: 'apps'
    }
  },
  computed: {
    weexSrc () {
      return getJS(`app/tab${navs.indexOf(this.index) + 1}/index.js`)
    }
  },
  components: {
    Tabbar, TabbarItem
  },
  methods: {
    getJS (path) {
      return `${weex.config.env.jsServer}/fe/dist/js/pages/${path}`
    }
  }
}
</script>
<style lang="stylus" scoped>
.content
  flex 1
</style>
