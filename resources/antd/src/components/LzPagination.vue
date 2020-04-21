<template>
  <a-pagination
    ref="page"
    v-if="page"
    v-model="currentPage"
    :page-size-options="pageSizes"
    class="my-2"
    :total="page.total"
    show-size-changer
    show-quick-jumper
    :show-total="total => `共 ${total} 条`"
    :page-size.sync="perPage"
    @change="onChange"
    @showSizeChange="onSizeChange"
  />
</template>

<script>
export default {
  name: 'LzPagination',
  data() {
    return {
      currentPage: 1,
      perPage: 15,
    }
  },
  props: {
    page: Object,
    /**
     * 分页改变时，是否自动改变地址栏的 query string
     */
    autoPush: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    pageSizes() {
      const sizes = ['15', '30', '50', '100', '200']
      const perPage = this.page.per_page.toString()
      if (sizes.indexOf(perPage) === -1) {
        return [perPage, ...sizes]
      } else {
        return sizes
      }
    },
  },
  methods: {
    push({ page, perPage }) {
      const query = Object.assign({}, this.$route.query, {
        page: page || this.currentPage,
        per_page: perPage || this.perPage,
      })
      this.$router.push({
        path: this.$route.path,
        query,
      })
    },
    onSizeChange(current, perPage) {
      this.$emit('size-change', perPage)
      if (!this.autoPush) {
        return
      }

      // 切换每页数后，当前页置为 1
      this.currentPage = 1
      this.push({ page: 1, perPage })
    },
    onChange(page) {
      this.$emit('current-change', page)
      this.autoPush && this.push({ page })
    },
  },
  watch: {
    page: {
      handler(newVal) {
        if (!newVal) {
          return
        }
        this.currentPage = newVal.current_page
        this.perPage = newVal.per_page
        // 处理浏览器前进后退时, 分页器的当前页不对的问题
        // this.$nextTick(() => {
        //   this.$refs.page.internalCurrentPage = newVal.current_page
        // })
      },
      immediate: true,
    },
  },
}
</script>
