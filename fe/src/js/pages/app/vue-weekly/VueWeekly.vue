<template>
  <div class="vue-weekly">
    <status-bar></status-bar>
    <u-header><u-icon slot="left" color="#fff" @click="$router.back()">&#xe697;</u-icon>Vue Weekly<u-icon slot="right" color="#fff" @click="showPeriod=true">&#xe699;</u-icon></u-header>
    <list v-if="current!=-1" class="category-list">
      <cell>
        <image class="period-cover" :src="selectedPeriod.cover_url"></image>
      </cell>
      <cell v-for="category in selectedPeriod.info" class="category-item">
        <text class="category-title">{{category.type_name}}</text>
        <list class="post-list">
          <cell v-for="post in category.list" class="post-item" @click="goto(post.url)">
            <text class="post-title">{{post.title}}</text>
            <text class="post-desc">{{post.describe}}</text>
          </cell>
        </list>
      </cell>
    </list>
    <wxc-popup :show="showPeriod" @wxcPopupOverlayClicked="showPeriod=false" pos="right" width="280" :popup-style="{top:'46px'}">
      <text class="period-title">往期内容</text>
      <list class="period-list">
        <cell v-for="(period,index) in periods" :key="period._id" class="period-item"
          @click="selectPeriod(index)">
          <text class="period-item-text" :style="{'color':index===current?'#489cff':''}">{{period.title}}</text>
        </cell>
      </list>
    </wxc-popup>
  </div>
</template>
<script>
import 'Config'
import Header from 'Components/navigation/Header.vue'
import WxcPopup from 'Components/weex-ui/packages/wxc-popup'
import StatusBar from 'Components/navigation/StatusBar'
import Iconfont from 'Components/text/Iconfont'
const stream = weex.requireModule('stream')
export default {
  data () {
    return {
      periods: [],
      current: -1,
      showPeriod: false
    }
  },
  computed: {
    selectedPeriod () {
      return this.periods[this.current]
    }
  },
  components: {
    [Header.name]: Header,
    [Iconfont.name]: Iconfont,
    WxcPopup,
    StatusBar
  },
  methods: {
    selectPeriod (index) {
      this.current = index
    },
    goto (url) {
      this.$router.open({
        name: 'vue-weekly-web',
        params: {
          url
        }
      })
    }
  },
  created () {
    stream.fetch({
      method: 'GET',
      type: 'json',
      headers: {
        'content-type': 'application/json; charset=utf-8'
      },
      url: 'http://api.feweekly.vue-js.com:9090/api/weeklylist/list'
    }, res => {
      if (res.status === 200 && res.data.status === 1) {
        res.data.result.forEach(item => {
          try {
            item.info = JSON.parse(item.info)
          } catch (e) {
            this.$notice.toast({ message: `${item.title} 内容解析失败`})
          }
        })
        this.periods = res.data.result
        this.current = 0
      } else {
        this.$notice.toast({ message: '列表加载失败' })
      }
    })
  }
}
</script>
<style lang="stylus" scoped>
@import "../../../css/variable.styl"
@import "../../../css/function.styl"
.period-cover
  width 750px
  height 446px
.category-item
  padding 20px
.category-title
  padding 10px 0 10px 20px
  color $color-highlight
  font-size 28px
  border-bottom 2px solid $color-highlight
.post-item
  padding 20px
.post-title
  line-height 42px
  font-size 28px
  color $color-primary
.post-desc
  color $color-text-primary
  font-size 22px
  line-height 32px
.period-title
  height 88px
  padding 30px 40px
  color $color-text-highlight
  font-size 28px
  border-bottom(1px, solid, $color-text-disabled)
.period-list
  padding 20px
.period-item
  padding 32px 20px
.period-item-text
  color $color-text-minor
  font-size 28px
.selected-period
  color red
</style>
