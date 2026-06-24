<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import ProductCard from '../components/ProductCard.vue'

const router = useRouter()

const projects = [
  {
    id: 2, level: 4,
    title: '任務管理 App',
    description: '具備新增、刪除、拖曳排序與本地儲存功能的 Todo 應用程式，資料持久化於 localStorage。',
    image: import.meta.env.BASE_URL + 'todo-cover.jpg',
    tags: ['Vue 3', '拖曳排序', 'localStorage'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'todo.html',
  },
  {
    id: 3, level: 4,
    title: '天氣查詢儀表板',
    description: '串接 Open-Meteo API，依城市名稱即時顯示目前天氣與五日預報，支援攝氏 / 華氏切換。',
    image: import.meta.env.BASE_URL + 'weather-cover.jpg',
    tags: ['JavaScript', 'Fetch API', 'CSS Grid'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'weather.html',
  },
  {
    id: 4, level: 5,
    title: '神奇生物圖鑑',
    description: '報紙剪報風格的主題收藏館，可填寫調查表單、星級評分、即時統計與分類篩選，並用 Web Audio API 模擬蓋章音效。',
    image: import.meta.env.BASE_URL + 'biology-cover.jpg',
    tags: ['Vue 3', 'Web Audio API', 'RWD'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'biology.html',
  },
  {
    id: 5, level: 5,
    title: '城市旅遊導覽',
    description: '以 Vue Router 打造的多頁式旅遊導覽：城市列表、各城市景點、景點詳細頁，並用 Pinia 管理狀態。',
    image: import.meta.env.BASE_URL + 'city-travel-cover.jpg',
    tags: ['Vue 3', 'Vue Router', 'Pinia'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'city-travel/index.html',
  },
  {
    id: 6, level: 4,
    title: '色彩敏銳度測試',
    description: '純 JavaScript 打造的色弱小遊戲：在色塊中找出顏色不同的那一格，難度逐關提升，訓練色彩辨識力。',
    image: import.meta.env.BASE_URL + 'color-test-cover.jpg',
    tags: ['JavaScript', 'HTML', 'CSS'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'color-test.html',
  },
  {
    id: 7, level: 4,
    title: '會員資料小卡',
    description: '以 Vue 3 打造的會員資料卡，輸入即時雙向綁定，動態預覽概念藝術風格的個人化會員卡片。',
    image: import.meta.env.BASE_URL + 'member-card-cover.jpg',
    tags: ['Vue 3', 'HTML', 'CSS'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'member-card.html',
  },
  {
    id: 8, level: 4,
    title: 'Culture 藝文平台文化體驗改版提案',
    description: '從翻找藝文活動資料，到關鍵字一搜即中。以聚合搜尋為核心、個人化推薦為輔的團體改版提案。',
    image: import.meta.env.BASE_URL + 'culture-cover.jpg',
    tags: ['團體專案', 'Vue 3', 'Tailwind CSS', 'Pinia'],
    githubUrl: 'https://github.com/Digi-Pack/NCHU-11503-iCulture',
    demoUrl: 'https://nchu-11503-iculture.dev-hub.io/',
  },
]

const favorites = ref(new Set())
const favoriteCount = computed(() => favorites.value.size)

function handleViewDetail(id) {
  router.push('/projects/' + id)
}

function handleToggleFavorite(id) {
  const next = new Set(favorites.value)
  if (next.has(id)) { next.delete(id) } else { next.add(id) }
  favorites.value = next
}
</script>

<template>
  <div class="projects-view">
    <div class="section-header">
      <h2 class="section-title">我的作品 <span>MY PROJECTS</span></h2>
      <span class="fav-badge" v-if="favoriteCount > 0">❤️ 已收藏 {{ favoriteCount }} 個</span>
    </div>
    <div class="projects-list">
      <ProductCard
        v-for="project in projects"
        :key="project.id"
        v-bind="project"
        :isFavorited="favorites.has(project.id)"
        @view-detail="handleViewDetail"
        @toggle-favorite="handleToggleFavorite"
      />
    </div>
  </div>
</template>

<style scoped>
.projects-view {
  max-width: 680px;
  margin: 0 auto 2rem;
  padding: 0 1rem;
}

.section-header {
  border-bottom: 3px solid #2A1F0E;
  margin-bottom: 1.25rem;
  padding-bottom: 0.35rem;
  display: flex;
  align-items: baseline;
  justify-content: space-between;
}

.section-title {
  margin: 0;
  font-size: 1.45rem;
  font-weight: 900;
}

.section-title span {
  font-size: 0.95rem;
  font-weight: 500;
  opacity: 0.65;
  margin-left: 0.4rem;
}

.fav-badge {
  font-size: 0.82rem;
  font-weight: 700;
  color: #C0392B;
}

.projects-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}
</style>
