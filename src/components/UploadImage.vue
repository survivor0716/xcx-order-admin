<template>
  <el-upload
    class="avatar-uploader"
    :action="api"
    :show-file-list="false"
    :on-preview="handlePreview"
    :on-success="handleAvatarScucess"
    :before-upload="beforeAvatarUpload">
    <img v-if="imageUrl" :src="imageUrl" class="avatar">
    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
  </el-upload>
</template>

<style scoped>
  .avatar-uploader {
    width: 178px;
    margin: 0 auto;
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader:hover {
    border-color: #20a0ff;
  }

  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
    margin: 0 auto;
  }
</style>

<script>
import config from '../../config'

export default {
  props: ['imageUrl'],
  computed: {
    api () {
      return config.build.api + '/upload'
      // return config.build.api + '/upload'
    }
  },
  data () {
    return {

    }
  },
  methods: {
    handleAvatarScucess (res, file) {
      this.imageUrl = URL.createObjectURL(file.raw)
      // console.log('upload success', res)
      this.$emit('setImageUrl', res.data.file)
    },
    beforeAvatarUpload (file) {
      const isJPG = file.type === 'image/jpeg'
      const isPNG = file.type === 'image/png'
      const isLt2M = file.size / 1024 / 1024 < 2

      if (!isJPG && !isPNG) {
        this.$message.error('上传头像图片只能是 JPG 或 PNG 格式!')
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!')
      }
      return (isJPG || isPNG) && isLt2M
    },
    handlePreview (file) {
      console.log('handle preview', file)
    }
  }
}
</script>
