<template>
  <div>
    <content-header/>
    <div class="content-main">
      <lz-form
        ref="form"
        :get-data="getData"
        :submit="onSubmit"
        :form.sync="form"
        :errors.sync="errors"
      >
        <lz-form-item label="标识" required prop="slug">
          <a-input v-model="form.slug"/>
        </lz-form-item>
        <lz-form-item label="名称" required prop="name">
          <a-input v-model="form.name"/>
        </lz-form-item>
        <lz-form-item label="方法" prop="http_method">
          <a-select v-model="form.http_method" mode="multiple">
            <a-select-option v-for="i of methods" :key="i">{{ i }}</a-select-option>
          </a-select>
        </lz-form-item>
        <!--<lz-form-item label="方法" prop="http_method">
          <el-select
            v-model="form.http_method"
            clearable
            multiple
            placeholder="选择请求方法"
          >
            <el-option
              v-for="i in methods"
              :key="i"
              :label="i"
              :value="i"
            />
          </el-select>
        </lz-form-item>-->
        <lz-form-item label="路径" prop="http_path">
          <a-input :auto-size="{ minRows: 5, maxRows: 5 }" type="textarea" v-model="form.http_path"/>
        </lz-form-item>
      </lz-form>
    </div>
  </div>
</template>

<script>
import {
  editAdminPerm,
  storeAdminPerm,
  updateAdminPerm,
} from '@/api/admin-perms'
import LzForm from '@c/LzForm'
import ContentHeader from '@c/ContentHeader'
import LzFormItem from '@c/LzForm/LzFormItem'

export default {
  name: 'Form',
  components: {
    LzForm,
    ContentHeader,
    LzFormItem,
  },
  data() {
    return {
      form: {
        slug: '',
        name: '',
        http_method: [],
        http_path: '',
      },
      errors: {},
      methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS', 'HEAD'],
    }
  },
  methods: {
    async getData() {
      if (this.$form.realEditMode) {
        const { data } = await editAdminPerm(this.resourceId)
        data.http_path = data.http_path.join('\n')
        return data
      }
    },
    async onSubmit() {
      if (this.$form.realEditMode) {
        await updateAdminPerm(this.resourceId, this.form)
      } else {
        await storeAdminPerm(this.form)
      }
    },
  },
}
</script>
