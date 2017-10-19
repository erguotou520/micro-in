<template>
  <div class="progress" :style="{height: barHeight}">
    <div class="line" ref="progress" :style="{backgroundColor: backgroundColor}"></div>
    <div class="completed" ref="completed" :style="{backgroundColor: barColor}"></div>
    <text v-if="showPercentage" class="text">{{percentage|textFilter}}%</text>
  </div>
</template>
<script>
const animation = weex.requireModule('animation')
const dom = weex.requireModule('dom')
export default {
  name: 'u-progress',
  props: {
    percentage: {
      type: Number,
      required: true,
      validator (val) {
        return val >= 0 && val <= 100
      }
    },
    barColor: String,
    barHeight: String,
    backgroundColor: String,
    showPercentage: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      progressWidth: 0
    }
  },
  watch: {
    percentage (val) {
      this.$nextTick(this.animateCompleted)
    }
  },
  filters: {
    textFilter (val) {
      return Math.floor(val * 10) / 10
    }
  },
  methods: {
    animateCompleted () {
      animation.transition(this.$refs.completed, {
        duration: 300,
        timingFunction: 'ease-in-out',
        styles: {
          width: `${this.progressWidth * this.percentage / 100}px`
        }
      })
    }
  },
  mounted () {
    this.$nextTick(() => {
      dom.getComponentRect(this.$refs.progress, option => {
        this.progressWidth = option.size.width
        this.animateCompleted()
      })
    })
  }
}
</script>
<style lang="stylus" scoped>
@import "../../css/variable.styl"
.progress
  position relative
  flex-direction row
  align-items center
  height 32px
.line
  flex 1
  height 4px
  background-color $color-text-disabled
  border-radius 2px
.completed
  position absolute
  left 0
  top 14px
  height 4px
  background-color $color-primary
  border-radius 2px
.text
  width 80px
  text-align right
  color $color-text-highlight
  font-size 24px
</style>
