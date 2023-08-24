<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'
import CategoryPanel from './components/CategoryPanel.vue'
import type { BannerItem, CategoryItem, HotItem } from '../../types/home'
import { HotPanel } from './components/HotPanel.vue'
import type { XtxGuessInstance } from '../../components/components'

const bannerList = ref<BannerItem[]>([])
const categoryList = ref<CategoryItem[]>([])
const hotItemList = ref<HotItem[]>([])

const getBannerList = async () => {
  const res = await getHomeBannerAPI()
  bannerList.value = res.result
}

const getCategoryList = async () => {
  const res = await getHomeCategoryAPI()
  categoryList.value = res.result
}

const getHotItemList = async () => {
  const res = await getHomeHotAPI()
  hotItemList.value = res.result
  console.log('res', res)
}

onLoad(async (option) => {
  getBannerList()
  getCategoryList()
  getHotItemList()
})

const guessRef = ref<XtxGuessInstance>()

// 滚动触底事件
const onScrolltolower = () => {
  guessRef.value?.getMore()
}
</script>

<template>
  <view class="index">
    <CustomNavbar />
    <scroll-view class="scroll-view" scroll-y refresher-enabled @scrolltolower="onScrolltolower">
      <XtxSwiper :list="bannerList" />
      <CategoryPanel :list="categoryList" />
      <HotPanel :list="hotItemList" />
      <XtxGuess ref="guessRef" />
    </scroll-view>
  </view>
</template>

<style lang="scss" scoped>
.index {
  background-color: #f7f7f7;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.scroll-view {
  flex: 1;
}
</style>
