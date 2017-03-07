<template>
  <div class="home">
    <dishes-type @updateDetail="updateDetail" ref="dishesType"></dishes-type>
    <dishes-detail :tableData="dishesDetailData" :typeId="currentTypeId" @refetchType="refetchType"></dishes-detail>
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
      console.log('$emit to Home.vue', detail, id)
      this.dishesDetailData = detail
      this.currentTypeId = id
    },
    refetchType () {
      this.$refs.dishesType.getType()
    }
  }
}
</script>

<style scoped lang="less">
.home {

}
</style>
