<template>
  <div class="home">
    <dishes-type @updateDetail="updateDetail"></dishes-type>
    <dishes-detail :tableData="dishesDetailData" :typeId="currentTypeId"></dishes-detail>
  </div>
</template>

<script>
import DishesType from '@/components/DishesType'
import DishesDetail from '@/components/DishesDetail'

export default {
  name: 'home',
  components: {
    DishesType,
    DishesDetail
  },
  data () {
    return {
      dishesDetailData: null,
      currentTypeId: null
    }
  },
  methods: {
    updateDetail (detail, id) {
      this.dishesDetailData = detail
      this.currentTypeId = id
    }
  }
}
</script>

<style scoped lang="less">
.home {

}
</style>
