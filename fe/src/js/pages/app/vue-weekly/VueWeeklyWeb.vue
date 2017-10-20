<template>
  <u-web :url="src" title="Vue Weekly"></u-web>
</template>
<script>
import WebViewer from 'Components/web/WebViewer'
const modal = weex.requireModule('modal')
const router = weex.requireModule('bmRouter')
export default {
  data () {
    return {
      src: ''
    }
  },
  components: {
    [WebViewer.name]: WebViewer
  },
  created () {
    router.getParams(params => {
      this.src = params.url
    })
  }
}
</script>
