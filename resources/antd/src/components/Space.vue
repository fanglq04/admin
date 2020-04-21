<script>
export default {
  name: 'Space',
  props: {
    size: Number,
  },
  render(h) {
    const slots = this.$slots.default || []
    const wrappedSlots = slots.map((i) => {
      return (<div class="space-item">{i}</div>)
    })

    return <div class="space">{wrappedSlots}</div>
  },
}
</script>

<style scoped lang="less">
.space {
  display: inline-flex;
}

.space-item + .space-item {
  margin-left: 8px;
}
</style>
