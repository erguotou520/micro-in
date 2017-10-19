<template>
  <div class="web-container">
    <status-bar></status-bar>
    <u-header><div slot="left" style="flex-direction:row"><u-icon v-if="showBack" color="#fff" @click="back">&#xe697;</u-icon><u-icon v-if="showClose" color="#fff" @click="close" style="margin-left:24px">&#xe69a;</u-icon><u-icon v-if="showReload" color="#fff" @click="reload" style="margin-left:24px">&#xe6aa;</u-icon></div>{{title}}<u-icon slot="right" color="#fff" @click="share">&#xe718;</u-icon></u-header>
    <u-progress v-if="percentage>0&&percentage<100" :percentage="percentage" background-color="transparent" bar-color="#3b8fce" bar-height="4px"></u-progress>
    <web ref="webview" :src="url" class="webview" @pagestart="start" @pagefinish="finish" @error="error"></web>
  </div>
</template>
<script>
import Header from 'Components/navigation/Header.vue'
import StatusBar from 'Components/navigation/StatusBar'
import Iconfont from 'Components/text/Iconfont'
import Progress from 'Components/data/Progress'
const webview = weex.requireModule('webview')
const share = weex.requireModule('bmShare')
const modal = weex.requireModule('modal')
export default {
  name: 'u-web',
  props: {
    url: String,
    title: String,
    // 用于share
    image: String,
    showBack: {
      type: Boolean,
      default: true
    },
    showClose: {
      type: Boolean,
      default: true
    },
    showReload: {
      type: Boolean,
      default: false
    },
    showShare: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      percentage: 0
    }
  },
  components: {
    [Header.name]: Header,
    [Progress.name]: Progress,
    [Iconfont.name]: Iconfont,
    StatusBar
  },
  methods: {
    back () {
      webview.goBack(this.$refs.webview)
    },
    share () {
      share.share({
        title: this.url,
        content: '',
        url: this.url,
        image: this.image,
      }, resData => {
        modal.toast({ message: '分享成功' })
      }, resData => {
        modal.toast({ message: '分享失败' })
      })
    },
    close () {
      this.$router.back()
    },
    reload (event) {
      webview.reload(this.$refs.webview)
    },
    start () {
      setInterval(() => {
        this.percentage += Math.random() * (100 - this.percentage) * 0.2
      }, 1000)
    },
    finish () {
      // 加载结束
      this.percentage = 99.99
      setTimeout(() => {
        this.percentage = 100
      }, 300)
    },
    error () {

    }
  }
}
</script>
<style lang="stylus" scoped>
.webview
  flex 1
</style>
