<template>
  <text :style="style"
    @click="e=>$emit('click', e)"
    @longpress="e=>$emit('longpress', e)"><slot></slot></text>
</template>
<script>
export default {
  name: 'u-icon',
  props: {
    size: {
      type: String,
      default: '34px'
    },
    color: {
      type: String,
      default: '#667180'
    },
    family: {
      type: String,
      default: 'iconfont'
    }
  },
  computed: {
    style () {
      return {
        fontFamily: this.family,
        color: this.color,
        fontSize: `${this.size}px`
      }
    }
  }
}
</script>
