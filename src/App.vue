<template>
  <div id="app">
    <el-menu :default-active="activeIndex" class="" mode="horizontal" @select="handleSelect" :router="true">
    <el-menu-item index="/">菜品管理</el-menu-item>
    <el-menu-item index="/config">其他设置</el-menu-item>
  </el-menu>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      activeIndex: '1'
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 1200px;
  margin: 0 auto
}
</style>
