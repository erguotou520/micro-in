<template>
  <div class="zhihu-daily">
    <status-bar></status-bar>
    <u-header>
      <div slot="left" style="flex-direction:row">
        <u-icon color="#fff" size="40px" @click="menuPopup=true" style="margin-right:40px">&#xe699;</u-icon>
        <text class="title" style="color:#fff;font-size:32px">首页</text>
      </div>
      <div slot="right" @click="morePopup=true">
        <u-icon color="#fff" size="40px">&#xe616;</u-icon>
      </div>
    </u-header>
    <wxc-ep-slider slider-id="top-stories" :card-length="topStories.length" style="margin-top:24px"
      :container-s="{position:'relative',width:750,height:450,overflow:'hidden'}"
      :card-s="{width:480,height:400,spacing:0,scale:0.6}" :select-index="0" ref="slider">
      <div v-for="(story,index) in topStories" :key="story.id" @click="goto(story)"
        :slot="`card${index}_top-stories`" :class="['slider',`slider${index}`]">
        <image class="top-story-image" :src="story.image"></image>
        <div class="top-story-mask">
          <text class="top-story-title">{{story.title}}</text>
        </div>
      </div>
    </wxc-ep-slider>
    <!-- <slider auto-play="true" interval="3000">
      <div v-for="(story,index) in topStories" :key="story.id" :class="['slider',`slider${index}`]">
        <image class="top-story-image" :src="story.image"></image>
        <text class="top-story-title">{{story.title}}</text>
      </div>
      <indicator item-color="#e6ecf2" item-selected-color="#ff8038" item-size="topStories.length"></indicator>
    </slider> -->
    <list>
      <cell v-for="story in stories" :key="story.id" class="story-cell" @click="goto(story)">
        <image class="story-image" :src="story.images?story.images[0]:''"></image>
        <text class="story-title">{{story.title}}</text>
      </cell>
    </list>
    <wxc-popup :show="menuPopup" pos="left" width="520" :popup-style="{top:'46px'}"></wxc-popup>
    <wxc-popup :show="morePopup" pos="right" width="240" :popup-style="{top:'46px'}"></wxc-popup>
  </div>
</template>
<script>
import 'Config'
import Header from 'Components/navigation/Header'
import Iconfont from 'Components/text/Iconfont'
import WxcEpSlider from 'Components/weex-ui/packages/wxc-ep-slider'
import WxcPopup from 'Components/weex-ui/packages/wxc-popup'
import StatusBar from 'Components/navigation/StatusBar'
const stream = weex.requireModule('stream')
export default {
  data () {
    return {
      morePopup: false,
      menuPopup: false,
      stories: [1],
      topStories: [2],
      slideIndex: 0,
      interval: null
    }
  },
  components: {
    [Header.name]: Header,
    [Iconfont.name]: Iconfont,
    WxcEpSlider,
    WxcPopup,
    StatusBar
  },
  methods: {
    goto (story) {
      this.$router.open({
        name: 'zhihu-daily-web',
        params: {
          story
        }
      })
    },
    loopSlider () {
      this.$refs.slider.slideTo(this.slideIndex, (this.slideIndex + 1) % this.topStories.length)
      this.slideIndex += 1
    }
  },
  created () {
    stream.fetch({
      method: 'GET',
      type: 'json',
      headers: {
        'content-type': 'application/json; charset=utf-8'
      },
      url: 'http://news-at.zhihu.com/api/4/news/latest'
    }, res => {
      if (res.status === 200) {
        this.stories = res.data.stories
        this.topStories = res.data.top_stories
        // this.$notice.alert({ message: res.data.stories instanceof Array, okTitle: 'ok'})
        this.interval = setInterval(this.loopSlider, 4000)
      } else {
        this.$notice.toast('数据加载失败:' + res.status)
      }
    })
  },
  beforeDestroy () {
    if (this.interval) {
      clearInterval(this.interval)
      this.interval = null
    }
  }
}
</script>
<style lang="stylus" scoped>
@import "../../../css/variable.styl"
.zhihu-daily
  background-color $color-background
.slider
  position relative
  justify-content: flex-end
  flex 1
.top-story-image
  position: absolute
  left 0
  top 0
  width 480px
  height 400px
.top-story-mask
  padding 20px
  background-color rgba(0, 0, 0, .3)
.top-story-title
  color #fff
  font-size 24px
.story-cell
  flex-direction: row
  margin-top 32px
  padding 20px
  background-color: #fff
  border-radius 8px
  box-shadow: 4px 4px 20px rgba(0, 0, 0, 0.5)
.story-image
  width 120px
  height 120px
  margin-right 20px
.story-title
  flex 1
  color $color-text-primary
  font-size 30px
</style>
