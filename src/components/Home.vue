<template>
  <div class="home">
    <dishes-type @updateDetail="updateDetail" ref="dishesType"></dishes-type>
    <dishes-detail :tableData="dishesDetailData" :type="currentType" :typeId="currentTypeId" @refetchType="refetchType" @refetchMenu="refetchMenu"></dishes-detail>
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
      currentType: null,
      currentTypeId: null
    }
  },
  methods: {
    updateDetail (detail, id, type) {
      // console.log('$emit to Home.vue', detail, id)
      this.dishesDetailData = detail
      this.currentTypeId = id
      if (type) {
        this.currentType = type
      }
    },
    refetchType () {
      // console.log('home refetchType')
      this.$refs.dishesType.getType()
    },
    refetchMenu (typeId) {
      // console.log('home refetchMenu', typeId)
      this.$refs.dishesType.getMenu(typeId)
    }
  }
}
</script>

<style scoped lang="less">
.home {
  width: 900px;
  margin: 0 auto;
}
</style>
