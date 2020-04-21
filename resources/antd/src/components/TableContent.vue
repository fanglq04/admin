<template>
  <div>
    <content-header v-bind="$props">
      <template #actions>
        <slot name="actions"/>
      </template>
    </content-header>
    <slot/>
    <div style="overflow-x: auto">
      <slot name="table"/>
    </div>
    <slot name="after"/>
  </div>
</template>

<script>
import ContentHeader from '@c/ContentHeader'

export default {
  name: 'TableContent',
  components: {
    ContentHeader,
  },
  props: {
    ...ContentHeader.props,
  },
}
</script>

<style scoped lang="less">

</style>
