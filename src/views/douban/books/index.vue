<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column label="ID" width="100" align="center">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>
      <el-table-column prop="title" label="书名" width="300" />
      <el-table-column prop="isbn" label="ISBN" width="140" />
      <el-table-column prop="pubdate" label="出版日期" width="180" />
      <el-table-column prop="rating" label="评分" width="60" align="center" />
      <el-table-column prop="series" label="简介" />
    </el-table>
    <pagination
      v-show="total > 0"
      :total="total"
      :page.sync="page"
      :limit.sync="limit"
      @pagination="fetchData"
    />
  </div>
</template>

<script>
import { getBook } from '@/api/book'
import Pagination from '@/components/Pagination'

export default {
  name: 'Books',
  components: { Pagination },
  data() {
    return {
      listLoading: true,
      page: 1,
      limit: 20,
      total: 0
    }
  },
  computed: {},
  created() {
    this.fetchData({
      page: this.page,
      limit: this.limit
    })
  },
  methods: {
    fetchData(params) {
      this.listLoading = true
      const { books, total } = getBook(params)
      this.list = books
      this.total = total
      this.listLoading = false
    }
  }
}
</script>

<style lang="scss">
  .block {
    padding: 15px 0;
  }
</style>
