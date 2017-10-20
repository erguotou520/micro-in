<template>
  <div class="app" :style="{height:appHeight}">
    <status-bar></status-bar>
    <embed :src="weexSrc" type="weex" class="content"></embed>
    <u-tabbar :value="index" @input="v=>index=v">
      <u-tabbar-item index="apps">&#xe601;</u-tabbar-item>
      <u-tabbar-item index="tools">&#xe65a;</u-tabbar-item>
      <u-tabbar-item index="search">&#xe6ac;</u-tabbar-item>
      <u-tabbar-item index="my">&#xe6b8;</u-tabbar-item>
    </u-tabbar>
  </div>
</template>
<script>
import 'Config'
import Tabbar from 'Components/navigation/Tabbar'
import TabbarItem from 'Components/navigation/TabbarItem'
import StatusBar from 'Components/navigation/StatusBar'
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
      appHeight: `${weex.config.env.deviceHeight * 750 / weex.config.env.deviceWidth}px`,
      index: 'apps'
    }
  },
  computed: {
    weexSrc () {
      return this.getJS(`app/Tab${navs.indexOf(this.index) + 1}.js`)
    }
  },
  components: {
    [Tabbar.name]: Tabbar,
    [TabbarItem.name]: TabbarItem,
    StatusBar
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
  color #00ff00
</style>
