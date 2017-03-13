<template>
  <div class="dish_detail">
    <h3>{{type}}</h3>
    <el-table
      :data="tableData"
      stripe
      v-loading="loading"
      element-loading-text="拼命加载中"
      style="width: 100%"
      height="350">
      <el-table-column
        prop="name"
        label="菜品名称"
        width="100">
      </el-table-column>
      <el-table-column
        prop="price"
        label="单价（元）"
        width="120">
      </el-table-column>
      <el-table-column
        prop="unit"
        label="单位"
        width="80">
      </el-table-column>
      <el-table-column
        prop="remark"
        label="备注"
        width="300">
      </el-table-column>
      <el-table-column
        prop="remark"
        label="状态"
        width="100">
        <template scope="scope">
          {{scope.row.state === 1 ? '正常' : scope.row.state === 2 ? '售罄' : '下架'}}
        </template>
      </el-table-column>
      <el-table-column
        prop="img"
        label="图片">
        <template scope="scope">
          <!-- <a v-if="scope.row.img" :href="scope.row.img" target="_blank">查看图片</a> -->
          <img :src="scope.row.img ? scope.row.img : img" alt="" style="height: 60px">
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
    <dialog-edit :rowData="rowData" :options="options" @refetchData="refetchData"></dialog-edit>
  </div>
</template>

<script>
import DialogEdit from '@/components/DialogDish'
import ComfirmMb from '@/components/DelDishMB'
import dishImg from '../assets/food.jpg'
// import config from '../../config'

export default {
  name: 'dish_detail',
  props: ['tableData', 'typeId', 'type'],
  components: {
    DialogEdit,
    ComfirmMb
  },
  data () {
    return {
      img: dishImg,
      loading: false,
      options: {
        title: null,
        type: null,
        typeId: this.typeId,
        menuId: null,
        dialogFormVisible: false
      },
      rowData: {
        name: null,
        price: null,
        unit: null,
        remark: null,
        state: null,
        img: null,
        sort: null
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
      this.rowData = row
    },
    showDialogAdd () {
      this.options = {
        title: '添加菜品',
        type: 'add',
        typeId: this.typeId,
        dialogFormVisible: true
      }
      this.rowData = {
        name: null,
        price: null,
        unit: null,
        remark: null,
        state: null,
        img: null,
        sort: null
      }
    },
    refetchData () {
      this.$emit('refetchMenu', this.typeId)
      this.$emit('refetchType')
    }
  },
  created () {

  }
}
</script>

<style scoped lang="less">
.dish_detail {
  width: 100%;
  text-align: left;
  margin-top: 20px;
}
</style>
