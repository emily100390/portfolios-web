<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import ProductCard from '../components/ProductCard.vue'

const router = useRouter()

const projects = [
  {
    id: 1, level: 5,
    title: '個人作品集網站',
    description: '以 Vue 3 + Vite 打造的響應式個人作品集，支援動態路由與深色模式。',
    image: 'https://picsum.photos/seed/portfolio/400/250',
    tags: ['Vue 3', 'Vite', 'RWD'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
  },
  {
    id: 2, level: 4,
    title: '任務管理 App',
    description: '具備新增、刪除、拖曳排序與本地儲存功能的 Todo 應用程式。',
    image: 'https://picsum.photos/seed/todo/400/250',
    tags: ['Vue 3', 'Pinia', 'localStorage'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
  },
  {
    id: 3, level: 4,
    title: '天氣查詢儀表板',
    description: '串接 OpenWeatherMap API，依城市名稱即時顯示天氣資訊與五日預報。',
    image: 'https://i.pinimg.com/1200x/0a/10/0c/0a100c37dbb1250297e60f5e565608e3.jpg',
    tags: ['JavaScript', 'Fetch API', 'CSS Grid'],
    githubUrl: 'https://github.com',
    demoUrl: 'https://example.com',
  },
  {
    id: 4, level: 5,
    title: '神奇生物圖鑑',
    description: '報紙剪報風格的主題收藏館，可填寫調查表單、星級評分、即時統計與分類篩選，並用 Web Audio API 模擬蓋章音效。',
    image: 'https://images.unsplash.com/photo-1541781774459-bb2af2f05b55?auto=format&fit=crop&w=400&q=80',
    tags: ['Vue 3', 'Web Audio API', 'RWD'],
    githubUrl: 'https://github.com',
    demoUrl: import.meta.env.BASE_URL + 'biology.html',
  },
  {
    id: 5, level: 5,
    title: '城市旅遊導覽',
    description: '以 Vue Router 打造的多頁式旅遊導覽：城市列表、各城市景點、景點詳細頁，並用 Pinia 管理狀態。',
    image: 'https://images.unsplash.com/photo-1502602898657-3e91760cbb34?auto=format&fit=crop&w=400&q=80',
    tags: ['Vue 3', 'Vue Router', 'Pinia'],
    githubUrl: 'https://github.com',
    demoUrl: import.meta.env.BASE_URL + 'city-travel/index.html',
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
