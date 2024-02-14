<script setup>
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import { onMounted } from 'vue'

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

  const loader = new GLTFLoader()

  loader.load('gltf/scene.gltf', (gltf) => {
    scene.add(gltf.scene)
  }, undefined, (error) => {
    console.error(error)
  })

  function animate() {
    requestAnimationFrame(animate)

    // required if controls.enableDamping or controls.autoRotate are set to true
    controls.update()
    renderer.render(scene, camera)
  }

  animate()
  // 监听窗口大小变化
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  })
})
</script>
