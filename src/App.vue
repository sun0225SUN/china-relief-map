<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'

const isLoading = ref(true)

const nowTime = ref('')

onMounted(() => {
  setInterval(() => {
    nowTime.value = new Date().toLocaleString()
  })
  const renderer = new THREE.WebGLRenderer()
  renderer.setSize(window.innerWidth, window.innerHeight)
  document.body.appendChild(renderer.domElement)

  const scene = new THREE.Scene()

  const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 10000)

  const controls = new OrbitControls(camera, renderer.domElement)

  // controls.update() must be called after any manual changes to the camera's transform
  camera.position.set(0, 20, 100)
  controls.update()

  const loader = new GLTFLoader()

  loader.load('gltf/scene.gltf', (gltf) => {
    scene.add(gltf.scene)
  }, cancelTip, (error) => {
    console.error(error)
  })

  function animate() {
    requestAnimationFrame(animate)

    // required if controls.enableDamping or controls.autoRotate are set to true
    controls.update()
    renderer.render(scene, camera)
  }

  animate()

  function cancelTip() {
    // 10s 后取消加载提示
    setTimeout(() => {
      isLoading.value = false
    }, 10000)
  }
})
</script>

<template>
  <div absolute w-full top-2>
    <div text-3r text-red text-center>
      北京时间：{{ nowTime }}
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
</template>

<style lang="scss" scoped>
</style>
