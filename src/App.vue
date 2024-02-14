<script setup>
import { onMounted, ref } from 'vue'
import Map from './components/map.vue'

const isLoading = ref(true)

const nowTime = ref('')

onMounted(() => {
  setInterval(() => {
    nowTime.value = new Date().toLocaleString()
  })
  setTimeout(() => {
    isLoading.value = false
  }, 5000)
})
</script>

<template>
  <Map />
  <div absolute w-full top-8>
    <div text-4 text-white text-center select-none>
      我国地形图立体展示 -- By 小孙同学
    </div>
  </div>
  <div v-if="isLoading" bg-black text-white flex h-100vh w-full justify-center items-center>
    <div>
      <div text-8>
        亲爱的，请耐心等待一下，让数据飞一会儿......
      </div>
      <div text-8>
        大概需要半分钟的时间，黑屏也是在加载中，不要离开！！！
      </div>
    </div>
  </div>
  <div absolute w-full bottom-5>
    <div text-3 text-red text-center select-none>
      当前时间：{{ nowTime }}
    </div>
  </div>
</template>

<style lang="scss" scoped>
</style>
