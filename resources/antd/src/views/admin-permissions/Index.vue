<template>
  <page-content>
    <space class="my-1">
      <search-form :fields="search"/>
    </space>

    <a-table
      row-key="id"
      :data-source="perms"
      bordered
      :scroll="{ x: 1200, y: 500 }"
      :pagination="false"
    >
      <a-table-column title="ID" data-index="id" :width="60"/>
      <a-table-column title="名称" data-index="name" :width="150"/>
      <a-table-column title="标识" data-index="slug" :width="150"/>
      <a-table-column title="路由">
        <template #default="record">
          <route-show :data="record"/>
        </template>
      </a-table-column>
      <a-table-column title="添加时间" data-index="created_at" :width="180"/>
      <a-table-column title="修改时间" data-index="updated_at" :width="180"/>
      <a-table-column title="操作" :width="100">
        <template #default="record">
          <space>
            <router-link :to="`/admin-permissions/${record.id}/edit`">编辑</router-link>
            <a-popconfirm
              title="确认删除？"
              @confirm="() => {}"
            >
              <a class="red-6" href="javascript:void(0);">删除</a>
            </a-popconfirm>
          </space>
        </template>
      </a-table-column>
    </a-table>
    <lz-pagination :page="page"/>
  </page-content>
</template>

<script>
import { getAdminPerms } from '@/api/admin-perms'
import RouteShow from './components/RouteShow'
import Space from '@c/Space'
import LzPagination from '@c/LzPagination'
import PageContent from '@c/PageContent'
import SearchForm from '@c/SearchForm'
// import RowDestroy from '@c/LzTable/RowDestroy'

export default {
  name: 'Index',
  components: {
    PageContent,
    LzPagination,
    Space,
    // RowDestroy,
    SearchForm,
    RouteShow,
  },
  data() {
    return {
      perms: [],
      page: null,

      search: [
        {
          field: 'id',
          label: 'ID',
        },
        {
          field: 'name',
          label: '名称',
        },
        {
          field: 'slug',
          label: '标识',
        },
        {
          field: 'http_path',
          label: '请求路径',
        },
      ],
    }
  },
  watch: {
    $route: {
      async handler(newVal) {
        const { data: { data, meta } } = await getAdminPerms(newVal.query)
        this.perms = data
        this.page = meta
      },
      immediate: true,
    },
  },
}
</script>
