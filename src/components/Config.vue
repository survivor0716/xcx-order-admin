<template>
  <div class="config">
    <h3>其他设置</h3>
    <el-form :model="form" ref="form" label-width="100px" label-position="left" class="config-form">
      <el-form-item
        label="打印份数"
        prop="copy"
        :rules="rules"
      >
        <el-input type="number" v-model.number="form.copy" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('form')">提交</el-button>
        <el-button @click="resetForm('form')">重置</el-button>
      </el-form-item>
    </el-form>
    <h4>按桌号生成二维码</h4>
    <el-form  :inline="true" :model="form" ref="qrForm" label-width="80px" label-position="left" class="qr-form">
      <el-form-item
        label="起始桌号"
        prop="beginNo"
      >
        <el-input type="number" v-model.number="qrForm.beginNo" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item
        label="结束桌号"
        prop="endNo"
      >
        <el-input type="number" v-model.number="qrForm.endNo" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('qrForm')">生成</el-button>
        <el-button @click="">下载</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="tableData"
      stripe
      v-loading="loading"
      element-loading-text="拼命加载中"
      style="width: 40%">
      <el-table-column
        prop="tableNo"
        label="桌号"
        width="100">
      </el-table-column>
      <el-table-column
        prop="tableQrcode"
        label="二维码">
        <template scope="scope">
          <el-popover
            ref="popover"
            placement="right"
            width="40"
            trigger="click">
            <img src="../assets/food.jpg" alt="" style="width: 100%; height: 100%">
            <!-- <a :href="scope.row.qrcode" target="_blank" slot="reference">查看二维码</a> -->
            <span slot="reference">点击查看</span>
          </el-popover>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'config',
  components: {

  },
  data () {
    return {
      form: {
        copy: 1
      },
      rules: [
        {required: true, message: '打印份数不能为空'},
        {type: 'number', message: '打印份数必须为数字值'}
      ],
      qrForm: {
        beginNo: 1,
        endNo: 1
      },
      qrRules: {
        beginNo: [
          {type: 'number', message: '桌号必须为数字值'}
        ],
        endNo: [
          {type: 'number', message: '桌号必须为数字值'}
        ]
      },
      tableData: [
        {
          tableNo: 1,
          tableQrcode: ''
        },
        {
          tableNo: 2,
          tableQrcode: ''
        },
        {
          tableNo: 3,
          tableQrcode: ''
        },
        {
          tableNo: 4,
          tableQrcode: ''
        }
      ],
      loading: false
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<style scoped lang="less">
.config {
  width: 100%;
  padding: 24px;
  .config-form {
    width: 300px;
  }
}
</style>
