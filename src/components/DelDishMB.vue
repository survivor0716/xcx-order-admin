<template>
  <el-button size="small" type="danger" icon="delete" @click="open2">删除</el-button>
</template>

<script>
import config from '../../config'

export default {
  props: ['row'],
  methods: {
    open2 () {
      this.$confirm('是否删除该菜品, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.handleDelDish()
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    handleDelDish () {
      var api = config.build.api
      // var api = config.dev.api
      var url = api + '/delMenu'
      // var url = 'https://order.jrhs.new-sailing.com/delMenu'
      var params = {
        menuId: this.row.id
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
          this.$message({
            type: 'success',
            message: '菜品已删除'
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
