<template>
  <div class="zhihu-web">
    <text>123</text>
    <!-- <u-web :url="src" :title="title">
      <image class="story-image" :src="image"></image>
    </u-web> -->
  </div>
</template>
<script>
// import Vue from 'vue'
// import WebViewer from 'Components/web/WebViewer'
// const modal = weex.requireModule('modal')
// const router = weex.requireModule('bmRouter')
// const stream = weex.requireModule('stream')
// export default {
//   data () {
//     return {
//       image_source: '',
//       title: '',
//       image: '',
//       share_url: '',
//       section: [],
//       id: ''
//     }
//   },
//   components: {
//     [WebViewer.name]: WebViewer
//   },
//   created () {
//     router.getParams(params => {
//       this.src = params.story.url
//       stream.fetch({
//         method: 'GET',
//         type: 'json',
//         headers: {
//           'content-type': 'application/json; charset=utf-8'
//         },
//         url: `https://news-at.zhihu.com/api/4/news/${params.story.id}`
//       }, res => {
//         if (res.status === 200) {
//           Object.assign(this, res.data)
//         } else {
//           modal.toast('数据加载失败:' + res.status)
//         }
//       })
//     })
//   }
// }
</script>
<style lang="stylus" scoped>
.story-image
  width 750px
  height 450px
</style>
