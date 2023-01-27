<template>
  <div class="divCenter pt-20">
    <div
      id="threejs-container"
      class=""
      :style="
        windowWidth < 1280
          ? null
          : {
              'padding-left': paddingValue - 50 + 'px',
              'padding-right': paddingValue - 50 + 'px'
            }
      "
    />
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { RoomEnvironment } from 'three/examples/jsm/environments/RoomEnvironment.js'

export default {
  props: {
    paddingValue: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      hasUnintendedBehaviors: false,
      windowWidth: window.innerWidth
    }
  },
  mounted() {
    let isInDesktop = window.innerWidth > 1148
    const userPlatform = platform => {
      return platform ? 5 : 3
    }

    const camera = new THREE.PerspectiveCamera(75, 2, 0.1, 1000)
    camera.position.x = 30
    camera.position.y = 20
    camera.position.z = 30

    const loader = new GLTFLoader()
    loader.load(
      ' ./web/source/laptop.glb',
      function (gltf) {
        const model = gltf.scene

        model.position.set(0, 0, 6)
        model.scale.set(
          userPlatform(isInDesktop),
          userPlatform(isInDesktop),
          userPlatform(isInDesktop)
        )
        model.castShadow = true
        scene.add(model)
      },
      undefined,
      function (error) {
        this.$router.push('notfound')
      }
    )

    const container = document.getElementById('threejs-container')

    const renderer = new THREE.WebGLRenderer(
      { antialias: true },
      { alpha: true }
    )
    renderer.setPixelRatio(window.devicePixelRatio)

    renderer.outputEncoding = THREE.sRGBEncoding
    renderer.setSize(450, 450 / 2)
    renderer.shadowMap.enabled = true
    renderer.shadowMap.type = THREE.PCFSoftShadowMap
    container.appendChild(renderer.domElement)
    renderer.setClearColor(0x000000, 0)

    const pmremGenerator = new THREE.PMREMGenerator(renderer)

    const scene = new THREE.Scene()

    scene.environment = pmremGenerator.fromScene(
      new RoomEnvironment(),
      1
    ).texture
    const light = new THREE.DirectionalLight(0x404040, 1)
    light.position.set(15, 20, 0)
    light.target.position.set(0, 0, 0)
    light.castShadow = true

    light.shadow.mapSize.width = 512
    light.shadow.mapSize.height = 512

    scene.add(light)
    scene.add(light.target)

    const controls = new OrbitControls(camera, renderer.domElement)

    controls.enableDamping = true
    controls.maxPolarAngle = Math.PI / 2 - 0.3
    controls.minDistance = 10
    controls.maxDistance = 50

    controls.enableRotate = true
    controls.rotateSpeed = 0.3

    controls.autoRotate = true
    controls.autoRotateSpeed = 1.5

    const gridHelper = new THREE.GridHelper(200, 50)

    const helper = new THREE.DirectionalLightHelper(light, 5)

    const animate = () => {
      requestAnimationFrame(animate)

      const width = window.innerWidth / 2
      const height = window.innerHeight / 2
      camera.aspect = window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()
      renderer.setSize(width, height)

      controls.update()
      renderer.render(scene, camera)
    }
    animate()
  }
}
</script>

<style scoped>
#threejs-container {
  width: 100%;
  height: 50%;
  display: flex;
  justify-content: center;
  text-align: center;
}

#threejs-container > canvas {
  display: inline;
}

#loading {
  border: 10px solid #f3f3f3;
  border-top: 10px solid var(--text);
  border-radius: 50%;
  width: 80px;
  height: 80px;
  animation: spin 1s linear infinite;
}
</style>
