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

    <!-- 按鈕 -->
    <div class="btn-wrap">
      <button class="btn-more" @click="showExp = !showExp">
        {{ showExp ? '▲ 收起經歷' : '了解更多 ▼' }}
      </button>
    </div>

    <!-- 職涯經歷面板 -->
    <Transition name="slide-down">
      <div v-if="showExp" class="exp-panel">
        <div class="exp-header">— 職涯歷程 —</div>
        <div v-for="(exp, i) in experiences" :key="i" class="exp-item">
          <div class="exp-left">
            <div class="exp-icon">{{ exp.icon }}</div>
            <div class="exp-line" v-if="i < experiences.length - 1"></div>
          </div>
          <div class="exp-body">
            <div class="exp-period">{{ exp.period }}</div>
            <div class="exp-role">{{ exp.role }}</div>
            <div class="exp-company">@ {{ exp.company }}</div>
            <p class="exp-desc">{{ exp.desc }}</p>
          </div>
        </div>
      </div>
    </Transition>

  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

const showExp = ref(false)
const canvasRef = ref(null)

let renderer, scene, camera, animId, model

const CANVAS_SIZE = 300

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

const experiences = [
  {
    period: '2013 – 2016',
    role: '機械繪圖員',
    company: '機械設備製造業',
    desc: '使用 AutoCAD 進行 2D 零件圖與工程圖面製作，負責尺寸公差審核，配合工程師進行設計改善與文件管理。',
    icon: '⚙️',
  },
  {
    period: '2016 – 2025',
    role: '逐漸資深機械繪圖員',
    company: '精密零件加工廠',
    desc: '協助生產文件管理、跨部門溝通協調，累積製造流程與品質管控的實務經驗。',
    icon: '🔧',
  },
  {
    period: '2026 – 至今',
    role: '前端工程師（轉職進行中）',
    company: '自學 & 作品集累積',
    desc: '全力投入前端技術學習：Vue 3、JavaScript ES6+、RWD、Git，以實作專案驗證學習成果。',
    icon: '💻',
  },
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
  width: 320px;
  height: 320px;
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
  width: 300px;
  height: 300px;
  display: block;
  z-index: 1;
}

/* ── 按鈕 ── */
.btn-wrap {
  display: flex;
  justify-content: center;
}

.btn-more {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.42rem 1.2rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.86rem;
  font-family: inherit;
  box-shadow: 2px 2px 0 #2A1F0E;
  transition: all 0.1s;
}

.btn-more:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

/* ── 經歷面板 ── */
.exp-panel {
  width: 100%;
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 10px;
  padding: 1.25rem 1.5rem;
  box-shadow: 4px 4px 0 #2A1F0E;
}

.exp-header {
  text-align: center;
  font-weight: 900;
  font-size: 0.9rem;
  letter-spacing: 0.18em;
  color: #5A4A30;
  margin-bottom: 1.25rem;
}

.exp-item {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.1rem;
}

.exp-item:last-child { margin-bottom: 0; }

.exp-left {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
}

.exp-icon {
  width: 36px;
  height: 36px;
  background: #F2E6C9;
  border: 2px solid #2A1F0E;
  border-radius: 50%;
  box-shadow: 2px 2px 0 #2A1F0E;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
}

.exp-line {
  flex: 1;
  width: 2px;
  background: rgba(42, 31, 14, 0.18);
  margin: 5px 0;
  min-height: 20px;
}

.exp-period {
  font-size: 0.74rem;
  font-weight: 700;
  color: #E8883A;
  margin-bottom: 0.1rem;
}

.exp-role {
  font-size: 0.95rem;
  font-weight: 900;
  margin-bottom: 0.08rem;
}

.exp-company {
  font-size: 0.76rem;
  color: #7A6A50;
  margin-bottom: 0.3rem;
}

.exp-desc {
  margin: 0;
  font-size: 0.82rem;
  line-height: 1.55;
  color: #3D2B1F;
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

/* ── Transition ── */
.slide-down-enter-active,
.slide-down-leave-active {
  transition: opacity 0.28s ease, transform 0.28s ease;
}

.slide-down-enter-from,
.slide-down-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}
</style>
