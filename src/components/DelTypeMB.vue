<template>
  <el-button size="small" type="danger" icon="delete" @click="open2">删除</el-button>
</template>

<script>
  export default {
    props: ['row'],
    methods: {
      open2 () {
        this.$confirm('是否删除该菜品种类, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.handleDelType()
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      },
      handleDelType () {
        var url = 'https://order.jrhs.new-sailing.com/delType'
        var params = {
          typeId: this.row.id
        }
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
            this.$message({
              type: 'error',
              message: response.body.errMsg
            })
          } else {
            // console.log('success', response.body.data)
            this.$emit('refetchType')
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
