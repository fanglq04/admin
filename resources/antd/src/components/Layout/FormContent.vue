<template>
  <div>
    <content-header/>
    <div class="content-main">
      <slot/>
    </div>
  </div>
</template>

<script>
import ContentHeader from '@c/ContentHeader'

export default {
  name: 'FormContent',
  components: {
    ContentHeader,
  },
}
</script>
