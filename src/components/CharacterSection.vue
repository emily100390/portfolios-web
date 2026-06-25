<template>
  <div class="char-section">

    <!-- 3D 角色舞台 -->
    <div class="char-stage">
      <div class="ring ring-a"></div>
      <div class="ring ring-b"></div>
      <canvas ref="canvasRef" class="char-canvas"></canvas>

      <div
        v-for="trait in traits"
        :key="trait.label"
        class="trait-ball"
        :class="`trait-${trait.pos}`"
      >{{ trait.label }}</div>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

const canvasRef = ref(null)

let renderer, scene, camera, animId, model

const CANVAS_SIZE = 400

onMounted(() => {
  const canvas = canvasRef.value

  renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true })
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer.setSize(CANVAS_SIZE, CANVAS_SIZE)
  renderer.outputColorSpace = THREE.SRGBColorSpace

  scene = new THREE.Scene()

  camera = new THREE.PerspectiveCamera(45, 1, 0.01, 200)
  camera.position.set(0, 0, 5)

  scene.add(new THREE.AmbientLight(0xffffff, 2))
  const dir = new THREE.DirectionalLight(0xffffff, 2.5)
  dir.position.set(2, 4, 3)
  scene.add(dir)
  const fill = new THREE.DirectionalLight(0xffffff, 0.8)
  fill.position.set(-2, -1, -2)
  scene.add(fill)

  const loader = new GLTFLoader()
  loader.load(`${import.meta.env.BASE_URL}models/sw.glb`, (gltf) => {
    model = gltf.scene

    // 置中並 scale 讓最大邊 = 2
    const box = new THREE.Box3().setFromObject(model)
    const center = box.getCenter(new THREE.Vector3())
    const size = box.getSize(new THREE.Vector3())
    const maxDim = Math.max(size.x, size.y, size.z)

    model.position.sub(center)
    model.position.y -= 0.4
    model.scale.setScalar(2 / maxDim)

    scene.add(model)

    // 計算 scale 後的 bounding sphere，自動 fit 相機
    const scaledSphere = new THREE.Box3()
      .setFromObject(model)
      .getBoundingSphere(new THREE.Sphere())
    const fovRad = (camera.fov * Math.PI) / 180
    const dist = (scaledSphere.radius / Math.tan(fovRad / 2)) * 1.15
    camera.position.set(0, scaledSphere.center.y, dist)
    camera.lookAt(0, scaledSphere.center.y, 0)
  }, undefined, (err) => {
    console.error('[CharacterSection] 模型載入失敗:', err)
  })

  const animate = () => {
    animId = requestAnimationFrame(animate)
    if (model) model.rotation.y += 0.008
    renderer.render(scene, camera)
  }
  animate()
})

onBeforeUnmount(() => {
  cancelAnimationFrame(animId)
  renderer?.dispose()
})

const traits = [
  { label: '探索欲', pos: 'tl' },
  { label: '進化力', pos: 'tr' },
  { label: '實作派', pos: 'br' },
  { label: '好合作', pos: 'bl' },
]
</script>

<style scoped>
.char-section {
  max-width: 680px;
  margin: 3rem auto 2rem;
  padding: 0 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
  isolation: isolate;
}

/* ── 舞台 ── */
.char-stage {
  position: relative;
  width: 400px;
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.ring {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
}

.ring-a {
  width: 314px;
  height: 314px;
  border: 3px dashed rgba(232, 136, 58, 0.75);
  animation: spin-cw 7s linear infinite;
}

.ring-b {
  width: 272px;
  height: 272px;
  border: 2px dashed rgba(42, 31, 14, 0.22);
  animation: spin-ccw 5s linear infinite;
}

.char-canvas {
  width: 400px;
  height: 400px;
  display: block;
  z-index: 1;
}

/* ── 人格特質球 ── */
.trait-ball {
  position: absolute;
  width: 66px;
  height: 66px;
  border-radius: 50%;
  background: #FAF3E0;
  border: 2.5px solid #2A1F0E;
  box-shadow: 3px 3px 0 #2A1F0E;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.72rem;
  font-weight: 900;
  color: #2A1F0E;
  z-index: 2;
  pointer-events: none;
}

.trait-tl { top: 16px;    left: 16px;  animation: grow-in 0.5s 0.2s ease both, float-y 2.8s 0.7s ease-in-out infinite; }
.trait-tr { top: 16px;    right: 16px; animation: grow-in 0.5s 0.5s ease both, float-y 3.2s 1.0s ease-in-out infinite; }
.trait-br { bottom: 16px; right: 16px; animation: grow-in 0.5s 0.8s ease both, float-y 3.0s 1.3s ease-in-out infinite; }
.trait-bl { bottom: 16px; left: 16px;  animation: grow-in 0.5s 1.1s ease both, float-y 2.6s 1.6s ease-in-out infinite; }

/* ── 動畫 ── */
@keyframes spin-cw  { to { transform: rotate(360deg); } }
@keyframes spin-ccw { to { transform: rotate(-360deg); } }

@keyframes grow-in {
  from { transform: scale(0); opacity: 0; }
  to   { transform: scale(1); opacity: 1; }
}

@keyframes float-y {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-7px); }
}

/* ── RWD：小螢幕縮小 3D 舞台，避免超出畫面 ── */
@media (max-width: 360px) {
  .char-stage {
    width: 280px;
    height: 280px;
  }
  .char-canvas {
    width: 260px !important;
    height: 260px !important;
  }
  .ring-a { width: 274px; height: 274px; }
  .ring-b { width: 236px; height: 236px; }
  .trait-ball {
    width: 56px;
    height: 56px;
    font-size: 0.66rem;
  }
  .trait-tl, .trait-bl { left: 8px; }
  .trait-tr, .trait-br { right: 8px; }
  .trait-tl, .trait-tr { top: 8px; }
  .trait-bl, .trait-br { bottom: 8px; }
}
</style>
