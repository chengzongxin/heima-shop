<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'
import CategoryPanel from './components/CategoryPanel.vue'
import type { BannerItem, CategoryItem, HotItem } from '../../types/home'
import { HotPanel } from './components/HotPanel.vue'

const bannerList = ref<BannerItem[]>([])
const pannelList = ref<HotItem[]>([])
const hotItemList = ref<CategoryItem[]>([])

const getBannerList = async () => {
  const res = await getHomeBannerAPI()
  bannerList.value = res.result
}

const getPannelList = async () => {
  const res = await getHomeCategoryAPI()
  pannelList.value = res.result
}

const getHotItemList = async () => {
  const res = await getHomeHotAPI()
  hotItemList.value = res.result
  console.log('res', res)
}

onLoad(async (option) => {
  getBannerList()
  getPannelList()
  getHotItemList()
})
</script>

<template>
  <view class="index">
    <CustomNavbar />
    <XtxSwiper :list="bannerList" />
    <CategoryPanel :list="pannelList" />
    <HotPanel :list="hotItemList" />
  </view>
</template>

<style lang="scss">
//
</style>
