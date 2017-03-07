<template>
  <div class="dish_detail">
    <el-table
    :data="tableData"
    stripe
    v-loading="loading"
    element-loading-text="拼命加载中"
    style="width: 100%"
    height="400">
      <el-table-column
        prop="name"
        label="菜品名称">
      </el-table-column>
      <el-table-column
        prop="price"
        label="单价（元）">
      </el-table-column>
      <el-table-column
        prop="unit"
        label="单位">
      </el-table-column>
      <el-table-column
        prop="img"
        label="图片">
        <template scope="scope">
          <!-- <a v-if="scope.row.img" :href="scope.row.img" target="_blank">查看图片</a> -->
          <img v-if="scope.row.img" :src="scope.row.img" alt="" style="height: 60px">
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200">
        <template scope="scope">
          <el-button size="small" icon="edit" @click="showDialogEdit(scope.row)">编辑</el-button>
          <comfirm-mb :row="scope.row" @refetchData="refetchData"></comfirm-mb>
        </template>
      </el-table-column>
    </el-table>
    <el-button v-if="typeId" type="primary" size="small" icon="plus"  @click="showDialogAdd">添加</el-button>
    <dialog-edit :form="selectedRow" :options="options" @refetchData="refetchData"></dialog-edit>
  </div>
</template>

<script>
import DialogEdit from '@/components/DialogEdit'
import ComfirmMb from '@/components/ComfirmMB'

export default {
  name: 'dish_detail',
  props: ['tableData', 'typeId'],
  components: {
    DialogEdit,
    ComfirmMb
  },
  data () {
    return {
      loading: false,
      options: {
        title: null,
        type: null,
        typeId: this.typeId,
        menuId: null,
        dialogFormVisible: false
      },
      selectedRow: {
        name: null,
        price: null,
        unit: null,
        img: null
      }
    }
  },
  methods: {
    updateImg (imgUrl) {
      this.$emit('updateImg', imgUrl)
    },
    showDialogEdit (row) {
      this.options = {
        title: '编辑菜品',
        type: 'edit',
        typeId: this.typeId,
        menuId: row.id,
        dialogFormVisible: true
      }
      this.selectedRow = row
    },
    showDialogAdd () {
      this.options = {
        title: '添加菜品',
        type: 'add',
        typeId: this.typeId,
        dialogFormVisible: true
      }
      this.selectedRow = {
        name: null,
        price: null,
        unit: null,
        img: null
      }
    },
    refetchData () {
      // console.log(id)
      var url = 'https://order.jrhs.new-sailing.com/getMenu'
      var params = {typeId: this.typeId}
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
          // this.$emit('updateDetail', response.body.data, id)
          this.tableData = response.body.data
        }
      }, response => {
        // error callback
        console.log('error')
      })

      this.$emit('refetchType')
    }
  },
  created () {

  }
}
</script>

<style scoped lang="less">
.dish_detail {
  width: 900px;
  text-align: left;
  margin-top: 20px;
}
</style>
