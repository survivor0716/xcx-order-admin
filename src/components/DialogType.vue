<template>
  <div class="edit_type">
    <el-dialog :title="options.title" v-model="options.dialogFormVisible" size="tiny">
      <el-form :model="form" ref="form" :rules="rules">
        <el-form-item label="菜品名称" prop="name" :label-width="formLabelWidth">
          <el-input v-model="form.name" auto-complete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="options.dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @keyup.enter="submitForm('form')" @click="submitForm('form')">提 交</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import config from '../../config'

export default {
  props: ['rowToChild', 'options'],
  components: {

  },
  computed: {
    form () {
      return {
        name: this.rowToChild.name,
        sort: this.rowToChild.sort
      }
    }
  },
  data () {
    var validateName = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('菜品名称不能为空'))
      } else {
        callback()
      }
    }
    return {
      dialogTableVisible: false,
      dialogFormVisible: false,
      rules: {
        name: [
          { validator: validateName, trigger: 'blur' }
        ]
      },
      formLabelWidth: '120px'
    }
  },
  created () {
    // console.log(this.name)
    // console.log(this.sort)
    // this.form.name = this.name
    // this.form.sort = this.sort
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if (this.options.type === 'add') {
            this.handleAddType()
          }
          if (this.options.type === 'edit') {
            this.handleModifyType()
          }
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    handleAddType () {
      var api = config.build.api
      // var api = config.dev.api
      var url = api + '/addType'
      // var url = 'https://order.jrhs.new-sailing.com/addType'
      var params = {
        name: this.form.name
      }
      // console.log(params)
      this.$http.post(url, {}, {
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
          this.$emit('refetchType')
          this.options.dialogFormVisible = false
          this.$message({
            type: 'success',
            message: '添加菜品种类成功'
          })
        }
      }, response => {
        // error callback
        console.log('error')
      })
    },
    handleModifyType () {
      var api = config.build.api
      // var api = config.dev.api
      var url = api + '/modifyType'
      // var url = 'https://order.jrhs.new-sailing.com/modifyType'
      var params = {
        typeId: this.options.typeId,
        name: this.form.name
      }
      // console.log(params)
      this.$http.post(url, {}, {
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
          this.$emit('refetchType')
          this.options.dialogFormVisible = false
          this.$message({
            type: 'success',
            message: '编辑菜品种类成功'
          })
        }
      }, response => {
        // error callback
        console.log('error')
      })
    }
  }
}
</script>

<style scoped lang="less">
  .edit_type {
    display: inline-block;
  }
</style>
