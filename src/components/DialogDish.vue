<template>
  <div class="edit">
    <el-dialog :title="options.title" v-model="options.dialogFormVisible" size="small">
      <el-form :model="form" ref="form" :rules="rules" label-width="80px">
        <el-form-item label="菜品名称" prop="name">
          <el-input v-model="form.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="菜品单价" prop="price">
          <el-input v-model="form.price" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="单位" prop="unit">
          <el-input v-model="form.unit" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="备注" prop="remark">
          <el-input v-model="form.remark" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="状态">
          <el-radio-group v-model="currstate">
            <el-radio :label="1">正常</el-radio>
            <el-radio :label="2">售罄</el-radio>
            <el-radio :label="3">下架</el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>

      <upload-image :imageUrl="form.img" @setImageUrl="setImageUrl"></upload-image>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancelSubmit">取 消</el-button>
        <el-button type="primary" @click="submitForm('form')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import UploadImage from '@/components/UploadImage'
import config from '../../config'

export default {
  props: ['rowData', 'options'],
  components: {
    UploadImage
  },
  computed: {
    form () {
      return {
        name: this.rowData.name,
        price: this.rowData.price,
        unit: this.rowData.unit,
        remark: this.rowData.remark,
        state: this.rowData.state,
        sort: this.rowData.sort,
        img: this.rowData.img
      }
    },
    state () {
      return this.rowData.state
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
    var validatePrice = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('菜品单价不能为空'))
      } else {
        callback()
      }
    }
    var validateUnit = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('单位不能为空'))
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
        ],
        price: [
          { validator: validatePrice, trigger: 'blur' }
        ],
        unit: [
          { validator: validateUnit, trigger: 'blur' }
        ]
      },
      currstate: this.state
    }
  },
  watch: {
    state (val) {
      console.log('watch: ', val)
      this.currstate = val
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if (this.options.type === 'add') {
            this.handleAddDish()
          }
          if (this.options.type === 'edit') {
            this.handleModifyDish()
          }
          this.dialogFormVisible = false
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    cancelSubmit () {
      this.options.dialogFormVisible = false
    },
    handleAddDish () {
      var api = config.build.api
      // var api = config.dev.api
      var url = api + '/addMenu'
      var params = {
        name: this.form.name,
        price: this.form.price,
        unit: this.form.unit,
        remark: this.form.remark,
        state: this.currstate,
        img: this.form.img,
        typeId: this.options.typeId
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
          this.$emit('refetchData')
          this.options.dialogFormVisible = false
          this.$message({
            type: 'success',
            message: '添加菜品成功'
          })
        }
      }, response => {
        // error callback
        console.log('error')
      })
    },
    handleModifyDish () {
      var api = config.build.api
      // var api = config.dev.api
      var url = api + '/modifyMenu'
      // console.log(url)
      var params = {
        menuId: this.options.menuId,
        name: this.form.name,
        price: this.form.price,
        unit: this.form.unit,
        remark: this.form.remark,
        state: this.currstate,
        img: this.form.img,
        typeId: this.options.typeId
      }
      console.log(params)
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
          this.$emit('refetchData')
          this.options.dialogFormVisible = false
          this.$message({
            type: 'success',
            message: '编辑菜品成功'
          })
        }
      }, response => {
        // error callback
        console.log('error')
      })
    },
    setImageUrl (url) {
      this.form.img = url
    }
  }
}
</script>

<style scoped lang="less">
  .edit {
    display: inline-block;
  }
</style>
