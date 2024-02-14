<script setup>
import { onMounted, ref } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'

const isLoading = ref(true)

onMounted(() => {
  const renderer = new THREE.WebGLRenderer()
  renderer.setSize(window.innerWidth, window.innerHeight)
  document.body.appendChild(renderer.domElement)

  const scene = new THREE.Scene()

  const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 10000)

  const controls = new OrbitControls(camera, renderer.domElement)

  // controls.update() must be called after any manual changes to the camera's transform
  camera.position.set(0, 20, 100)
  controls.update()

  function animate() {
    requestAnimationFrame(animate)

    // required if controls.enableDamping or controls.autoRotate are set to true
    controls.update()

    renderer.render(scene, camera)
  }
  animate()

  function cancelTip() {
    isLoading.value = false
  }
  const loader = new GLTFLoader()

  loader.load('gltf/scene.gltf', (gltf) => {
    scene.add(gltf.scene)
  }, cancelTip, (error) => {
    console.error(error)
  })
})
</script>

<template>
  <div v-if="isLoading" bg-black text-white w-full>
    <h1>请耐心等待数据加载。。。</h1>
  </div>
</template>

<style lang="scss" scoped>
</style>
