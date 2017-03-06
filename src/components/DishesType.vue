<template>
  <div class="dish_type">
    <el-table
    :data="tableData"
    stripe
    style="width: 100%">
      <el-table-column
        prop="id"
        label="菜品id">
      </el-table-column>
      <el-table-column
        prop="name"
        label="菜品种类">
      </el-table-column>
      <el-table-column
        prop="total"
        label="拥有菜品数量">
      </el-table-column>
      <el-table-column label="操作">
        <template scope="scope">
        <el-button size="small" @click="getMenu(scope.row.id)">获取菜品列表</el-button>
        <!-- <el-button
          size="small"
          @click="handleEdit(scope.$index, scope.row)">编辑1</el-button> -->
          <dialog-type title="编辑菜品种类" :data="scope.row" @refetchType="getType" type="edit">编辑</dialog-type>
        <el-button
          size="small"
          type="danger"
          @click="handleDelete(scope.row.id, scope.$index, tableData)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <dialog-type title="添加菜品种类" data="" @refetchType="getType" type="add">添加</dialog-type>
  </div>
</template>

<script>
import DialogType from '@/components/DialogType'

export default {
  name: 'dish_type',
  components: {
    DialogType
  },
  data () {
    return {
      tableData: null
    }
  },
  methods: {
    handleEdit (index, row) {
      console.log(arguments)
    },
    handleDelete (id, index, rows) {
      var url = 'https://order.jrhs.new-sailing.com/delType'
      var params = {typeId: id}
      console.log(params)
      this.$http.get(url, {
        params: params,
        // use before callback
        before (request) {
          // abort previous request, if exists
          if (this.previousRequest) {
            this.previousRequest.abort()
          }

          // set previous request on Vue instance
          this.previousRequest = request
        }

      }).then(response => {
        // success callback
        if (response.body.errCode) {
          console.log('failed', response.body)
        } else {
          console.log('handled delete', response.body)
          // this.getType()
          rows.splice(index, 1)
        }
      }, response => {
        // error callback
        console.log('error')
      })
    },
    getType () {
      var url = 'https://order.jrhs.new-sailing.com/getType'
      this.$http.get(url, {
        // use before callback
        before (request) {
          // abort previous request, if exists
          if (this.previousRequest) {
            this.previousRequest.abort()
          }

          // set previous request on Vue instance
          this.previousRequest = request
        }

      }).then(response => {
        // success callback
        if (response.body.errCode) {
          console.log('failed', response.body)
        } else {
          console.log('got type', response.body.data)
          this.tableData = response.body.data
        }
      }, response => {
        // error callback
        console.log('error')
      })
    },
    getMenu (id) {
      // console.log(id)
      var url = 'https://order.jrhs.new-sailing.com/getMenu'
      var params = {typeId: id}
      this.$http.get(url, {
        params: params,
        // use before callback
        before (request) {
          // abort previous request, if exists
          if (this.previousRequest) {
            this.previousRequest.abort()
          }

          // set previous request on Vue instance
          this.previousRequest = request
        }

      }).then(response => {
        // success callback
        if (response.body.errCode) {
          console.log('failed', response.body)
        } else {
          // console.log('success', response.body.data)
          this.$emit('updateDetail', response.body.data, id)
          // this.tableData = response.body.data
        }
      }, response => {
        // error callback
        console.log('error')
      })
    }
  },
  created () {
    this.getType()
  }
}
</script>

<style scoped lang="less">
.dish_type {
  width: 600px;
  text-align: left;
}
</style>
