<template>
  <div class="dish_type">
    <el-table
    :data="tableData"
    stripe
    style="width: 100%"
    height="250">
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
          <!-- <dialog-type title="编辑菜品种类" :data="scope.row" @refetchType="getType" type="edit">编辑</dialog-type> -->
          <el-button size="small" icon="edit" @click="showDialogEdit(scope.row)">编辑</el-button>
          <del-type :row="scope.row" @refetchType="getType"></del-type>
        </template>
      </el-table-column>
    </el-table>
    <el-button type="primary" size="small" icon="plus" @click="showDialogAdd">添加</el-button>
    <dialog-type :form="selectedRow" :options="options" @refetchType="getType"></dialog-type>
  </div>
</template>

<script>
import DialogType from '@/components/DialogType'
import DelType from '@/components/DelTypeMb'

export default {
  name: 'dish_type',
  components: {
    DialogType,
    DelType
  },
  data () {
    return {
      tableData: null,
      options: {
        title: null,
        type: null,
        typeId: null,
        dialogFormVisible: false
      },
      selectedRow: {
        name: null
      }
    }
  },
  methods: {
    showDialogEdit (row) {
      this.options = {
        title: '编辑菜品种类',
        type: 'edit',
        typeId: row.id,
        dialogFormVisible: true
      }
      this.selectedRow = row
    },
    showDialogAdd () {
      this.options = {
        title: '添加菜品种类',
        type: 'add',
        typeId: null,
        dialogFormVisible: true
      }
      this.selectedRow = {
        name: null
      }
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
          // console.log('got type', response.body.data)
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
          console.log('getMenu response: ', response.body.data)
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
  width: 900px;
  text-align: left;
}
</style>
