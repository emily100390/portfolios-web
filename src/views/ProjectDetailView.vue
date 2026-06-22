<script setup>
import { computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route  = useRoute()
const router = useRouter()
const id     = route.params.id

const projects = [
  {
    id: 1,
    title: '個人作品集網站',
    description: '以 Vue 3 + Vite 打造的響應式個人作品集，支援動態路由與深色模式切換，並部署至 GitHub Pages。',
    image: 'https://picsum.photos/seed/portfolio/800/400',
    tags: ['Vue', 'CSS', 'API'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
    itinerary: [
      '規劃頁面架構與路由設計',
      '實作 NavBar、ProfileCard、SkillCard 等元件',
      '串接 GitHub API 動態載入 repo 列表',
      '撰寫 RWD 樣式並部署至 GitHub Pages',
    ],
   
  },
  {
    id: 2,
    title: '任務管理 App',
    description: '具備新增、刪除、拖曳排序與本地儲存功能的 Todo 應用程式，資料持久化於 localStorage。',
    image: 'https://picsum.photos/seed/todo/800/400',
    tags: ['Vue', 'CSS', 'API'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
    itinerary: [
      '設計 Pinia store 管理任務狀態',
      '實作新增、完成、刪除任務功能',
      '整合 vuedraggable 支援拖曳排序',
      '使用 localStorage 持久化資料',
    ],

  },
  {
    id: 3,
    title: '天氣查詢儀表板',
    description: '串接 OpenWeatherMap API，依城市名稱即時顯示天氣資訊與五日預報，支援攝氏 / 華氏切換。',
    image: 'https://i.pinimg.com/1200x/0a/10/0c/0a100c37dbb1250297e60f5e565608e3.jpg',
    tags: ['Vue', 'CSS', 'API'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
    itinerary: [
      '申請 OpenWeatherMap API 金鑰',
      '實作城市搜尋與即時天氣顯示',
      '繪製五日預報區塊（CSS Grid 排版）',
      '加入攝氏 / 華氏溫度切換功能',
    ],
  },
  {
    id: 4,
    title: '神奇生物圖鑑',
    description: '報紙剪報風格的主題收藏館：填寫調查表單、星級評分（危險度 / 友善度）、即時統計與分類篩選，並用 Web Audio API 模擬蓋章與撕毀音效。',
    image: 'https://images.unsplash.com/photo-1541781774459-bb2af2f05b55?auto=format&fit=crop&w=800&q=80',
    tags: ['Vue 3', 'Web Audio API', 'CSS'],
    githubUrl: 'https://github.com',
    demoUrl: import.meta.env.BASE_URL + 'biology.html',
    itinerary: [
      '設計報紙剪報風格的版面與 CSS 樣式',
      '實作收藏表單與星級評分元件',
      '用 computed 計算總數與平均危險 / 友善度',
      '加入分類篩選與 Web Audio API 音效',
    ],
  },
]

const project = computed(() =>
  projects.find(p => String(p.id) === String(id))
)
</script>

<template>
  <div class="detail-view">
    <button type="button" class="btn-back" @click="router.push('/projects')">← 回到作品列表</button>

    <div v-if="project">
      <img :src="project.image" :alt="project.title" class="hero-img" />

      <h1 class="project-title">{{ project.title }}</h1>

      <div class="tags">
        <span v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</span>
      </div>

      <div class="retro-card">
        <div class="section-label">▎專案介紹</div>
        <p>{{ project.description }}</p>
      </div>

      <div class="retro-card">
        <div class="section-label">▎開發流程</div>
        <ol class="list">
          <li v-for="(step, i) in project.itinerary" :key="i">{{ step }}</li>
        </ol>
      </div>

      <div class="link-row">
        <a :href="project.githubUrl" target="_blank" rel="noopener" class="link-btn">GitHub</a>
        <a :href="project.demoUrl"   target="_blank" rel="noopener" class="link-btn demo">Demo</a>
      </div>
    </div>

    <div v-else class="not-found">找不到 id 為「{{ id }}」的作品。</div>
  </div>
</template>

<style scoped>
.detail-view {
  max-width: 680px;
  margin: 1.5rem auto 3rem;
  padding: 0 1rem;
}

.btn-back {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.38rem 0.9rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.85rem;
  font-family: inherit;
  box-shadow: 2px 2px 0 #2A1F0E;
  margin-bottom: 1.25rem;
  transition: all 0.1s;
}

.btn-back:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

.hero-img {
  width: 100%;
  border: 2.5px solid #2A1F0E;
  border-radius: 10px;
  max-height: 320px;
  object-fit: cover;
  box-shadow: 4px 4px 0 #2A1F0E;
  margin-bottom: 1.25rem;
}

.project-title {
  font-size: 1.6rem;
  font-weight: 900;
  margin: 0 0 0.75rem;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-bottom: 1.25rem;
}

.tag {
  background: #FAF3E0;
  border: 1.5px solid #2A1F0E;
  border-radius: 4px;
  padding: 0.18rem 0.55rem;
  font-size: 0.78rem;
  font-weight: 600;
  box-shadow: 1px 1px 0 #2A1F0E;
}

.retro-card {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 10px;
  padding: 1rem 1.25rem;
  margin-bottom: 0.85rem;
  box-shadow: 3px 3px 0 #2A1F0E;
}

.section-label {
  font-weight: 900;
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
  color: #2A1F0E;
}

.retro-card p {
  margin: 0;
  font-size: 0.9rem;
  line-height: 1.6;
  color: #3D2B1F;
}

.list {
  margin: 0;
  padding-left: 1.3rem;
  font-size: 0.9rem;
  line-height: 2;
  color: #3D2B1F;
}

.link-row {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
}

.link-btn {
  flex: 1;
  text-align: center;
  padding: 0.55rem 0;
  border: 2px solid #2A1F0E;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 700;
  font-size: 0.9rem;
  background: #FAF3E0;
  color: #2A1F0E;
  box-shadow: 3px 3px 0 #2A1F0E;
  transition: all 0.1s;
}

.link-btn:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(3px, 3px);
}

.link-btn.demo {
  background: #E8883A;
  border-color: #2A1F0E;
  color: #FAF3E0;
}

.link-btn.demo:hover {
  background: #2A1F0E;
}

.not-found {
  text-align: center;
  color: #888;
  margin-top: 3rem;
}
</style>
