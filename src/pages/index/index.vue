<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import { getHomeBannerAPI, getHomeCategoryAPI, getHomeHotAPI } from '@/services/home'
import { onLoad } from '@dcloudio/uni-app'
import { ref } from 'vue'
import CategoryPanel from './components/CategoryPanel.vue'
import type { BannerItem, CategoryItem, HotItem } from '../../types/home'
import { HotPanel } from './components/HotPanel.vue'
import type { XtxGuessInstance } from '../../components/components'
import PageSkeleton from './components/PageSkeleton.vue'
import { useGuessList } from '@/composables'

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

// 下拉刷新状态
const isTriggered = ref(false)
// 加载状态
const isLoading = ref(false)

onLoad(async (option) => {
  isLoading.value = true
  await Promise.all([getBannerList(), getCategoryList(), getHotItemList()])
  isLoading.value = false
})

const onRefresherrefresh = async () => {
  // 开启动画
  isTriggered.value = true
  // 重置猜你喜欢组件数据
  guessRef.value?.resetData()
  // 加载数据
  await Promise.all([getBannerList(), getCategoryList(), getHotItemList()])
  // 关闭动画
  isTriggered.value = false
}

// 猜你喜欢组合式函数
const { guessRef, onScrolltolower } = useGuessList() // [!code ++]
</script>

<template>
  <view class="index">
    <CustomNavbar />

    <scroll-view
      class="scroll-view"
      scroll-y
      refresher-enabled
      :refresher-triggered="isTriggered"
      @refresherrefresh="onRefresherrefresh"
      @scrolltolower="onScrolltolower"
    >
      <PageSkeleton v-if="isLoading" />
      <template v-else>
        <XtxSwiper :list="bannerList" />
        <CategoryPanel :list="categoryList" />
        <HotPanel :list="hotItemList" />
        <XtxGuess ref="guessRef" />
      </template>
    </scroll-view>
  </view>
</template>

<style lang="scss" scoped>
.index {
  background-color: #f7f7f7;
  // height: 100%;
  // 这里不能用100%,否则会没有高度，scrollView也会没有高度，不会触发加载更多
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.scroll-view {
  flex: 1;
}
</style>
