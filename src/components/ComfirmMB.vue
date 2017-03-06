<template>
  <el-button size="small" type="danger" @click="open2">删除</el-button>
</template>

<script>
  export default {
    methods: {
      open2 () {
        this.$confirm('是否删除该菜品, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      }
    }
  }
</script>
