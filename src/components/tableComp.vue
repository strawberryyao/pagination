<template>
  <div>
    <el-input v-model="age" placeholder="age" @keyup.native="search"></el-input>
    <el-button type="primary" @click="search">查询</el-button>
    <el-table @row-click="fn" :data="tableData" style="width: 100%" highlight-current-row>
      <el-table-column prop="age" label="年龄" width="180">
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="180">
      </el-table-column>
    </el-table>
    <el-pagination @current-change="handleCurrentChange" background layout="prev, pager, next" :page-size="10" :total="allData.length">
    </el-pagination>
  </div>
</template>

<script>
//https://easy-mock.com/mock/5b87e88095c42459d24869f9/example/query
import axios from 'axios'
export default {
  async mounted() {
    let res = []
    for (let i = 1; i <= 101; i++) {
      let o = {
        name: 'aaa' + i,
        age: i
      }
      res.push(o)
    }
    this.allData = res
    let start = (this.pageNum - 1) * 10
    let end = this.pageNum * 10
    this.tableData = res.slice(start, end)
  },
  data() {
    return {
      age: '',
      row: {},
      allData: [],
      tableData: [],
      pageSize: 10,
      pageNum: 1
    }
  },
  methods: {
    fn(row, event, column) {
      this.row = row
    },
    search() {
      if (this.age) {
        let res = this.allData.filter(x => x.age == this.age)
        let start = (this.pageNum - 1) * 10
        let end = this.pageNum * 10
        this.tableData = res.slice(start, end)
      } else {
        this.tableData = this.allData
      }
    },
    handleCurrentChange(val) {
      this.pageNum = val
      let start = (this.pageNum - 1) * 10
      let end = this.pageNum * 10
      this.tableData = this.allData.slice(start, end)
    }
  }
}
</script>

<style>
</style>
