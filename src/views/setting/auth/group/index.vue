<template>
  <cs-container :is-back-to-top="true">
    <page-header
      slot="header"
      :loading="loading"
      @submit="handleSubmit"
      ref="header"/>

    <page-main
      :loading="loading"
      :table-data="table"
      @sort="handleSort"/>
  </cs-container>
</template>

<script>
import { getAuthGroupList } from '@/api/auth/group'

export default {
  name: 'setting-auth-group',
  components: {
    'PageHeader': () => import('./components/PageHeader'),
    'PageMain': () => import('./components/PageMain')
  },
  data() {
    return {
      table: [],
      loading: true,
      order: {
        order_type: undefined,
        order_field: undefined
      }
    }
  },
  mounted() {
    this.handleSubmit()
  },
  methods: {
    // 提交查询请求
    handleSubmit(form) {
      this.loading = true
      getAuthGroupList({
        ...form,
        ...this.order
      })
        .then(res => {
          this.table = res.data || []
        })
        .finally(() => {
          this.loading = false
        })
    },
    // 排序刷新
    handleSort(val) {
      this.order = val
      this.$nextTick(() => {
        this.$refs.header.handleFormSubmit()
      })
    }
  }
}
</script>
