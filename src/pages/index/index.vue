<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import { getHomeBannerAPI, getHomeCategoryAPI } from '@/services/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'
import CategoryPanel from './components/CategoryPanel.vue'
import type { BannerItem, CategoryItem } from '../../types/home'

const bannerList = ref<BannerItem[]>([])
const pannelList = ref<CategoryItem[]>([])

const getBannerList = async () => {
  const res = await getHomeBannerAPI()
  bannerList.value = res.result
}

const getPannelList = async () => {
  const res = await getHomeCategoryAPI()
  pannelList.value = res.result
}

onLoad(async (option) => {
  getBannerList()
  getPannelList()
})
</script>

<template>
  <view class="index">
    <CustomNavbar />
    <XtxSwiper :list="bannerList" />
    <CategoryPanel :list="pannelList" />
  </view>
</template>

<style lang="scss">
//
</style>
