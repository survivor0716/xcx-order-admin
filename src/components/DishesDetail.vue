<template>
  <div class="dish_detail">
    <el-table
    :data="tableData"
    stripe
    v-loading="loading"
    element-loading-text="拼命加载中"
    style="width: 100%">
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
      </el-table-column>
      <el-table-column label="操作" width="200">
        <template scope="scope">
          <!-- <el-button
            size="small"
            @click="handleEdit(scope.$index, scope.row)">编辑</el-button> -->
          <!-- <el-button
            size="small"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">删除</el-button> -->
          <dialog-edit title="编辑菜品" :data="scope.row" type="edit" :belongTo="typeId" @refetchData="refetchData">编辑</dialog-edit>
          <comfirm-mb></comfirm-mb>
        </template>
      </el-table-column>
    </el-table>
    <!-- <dialog-edit title="添加菜品" type="add" :belongTo="typeId" @refetchData="refetchData">添加</dialog-edit> -->
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
      loading: false
    }
  },
  methods: {
    updateImg (imgUrl) {
      this.$emit('updateImg', imgUrl)
    },
    refetchData (id) {
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
          // this.$emit('updateDetail', response.body.data, id)
          this.tableData = response.body.data
        }
      }, response => {
        // error callback
        console.log('error')
      })
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
