<script setup>
import { computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route  = useRoute()
const router = useRouter()
const id     = route.params.id

const projects = [
  {
    id: 2,
    title: '任務管理 App',
    description: '具備新增、刪除、拖曳排序與本地儲存功能的 Todo 應用程式，資料持久化於 localStorage。',
    image: import.meta.env.BASE_URL + 'todo-cover.jpg',
    tags: ['Vue 3', '拖曳排序', 'localStorage'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'todo.html',
    itinerary: [
      '用 Vue 3 響應式狀態管理任務清單',
      '實作新增、完成、刪除任務功能',
      '以原生 drag 事件支援拖曳排序',
      '使用 localStorage 持久化資料',
    ],

  },
  {
    id: 3,
    title: '天氣查詢儀表板',
    description: '串接 Open-Meteo 免金鑰天氣 API，依城市名稱即時顯示目前天氣與五日預報，支援攝氏 / 華氏切換。',
    image: import.meta.env.BASE_URL + 'weather-cover.jpg',
    tags: ['JavaScript', 'Fetch API', 'CSS Grid'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'weather.html',
    itinerary: [
      '用 Open-Meteo 地理編碼將城市名轉為經緯度',
      '串接 Fetch API 取得目前天氣與五日預報',
      '繪製五日預報區塊（CSS Grid 排版）',
      '加入攝氏 / 華氏溫度切換功能',
    ],
  },
  {
    id: 4,
    title: '神奇生物圖鑑',
    description: '報紙剪報風格的主題收藏館：填寫調查表單、星級評分（危險度 / 友善度）、即時統計與分類篩選，並用 Web Audio API 模擬蓋章與撕毀音效。',
    image: import.meta.env.BASE_URL + 'biology-cover.jpg',
    tags: ['Vue 3', 'Web Audio API', 'CSS'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'biology.html',
    itinerary: [
      '設計報紙剪報風格的版面與 CSS 樣式',
      '實作收藏表單與星級評分元件',
      '用 computed 計算總數與平均危險 / 友善度',
      '加入分類篩選與 Web Audio API 音效',
    ],
  },
  {
    id: 5,
    title: '城市旅遊導覽',
    description: '以 Vue Router 打造的多頁式旅遊導覽網站：城市列表、各城市景點、景點詳細頁，並用 Pinia 管理狀態。',
    image: import.meta.env.BASE_URL + 'city-travel-cover.jpg',
    tags: ['Vue 3', 'Vue Router', 'Pinia'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'city-travel/index.html',
    itinerary: [
      '設計城市列表、景點、詳細頁三層路由',
      '用動態路由參數傳遞城市與景點 id',
      '以 Pinia store 管理跨頁狀態',
      '撰寫各頁面樣式與 RWD 排版',
    ],
  },
  {
    id: 6,
    title: '色彩敏銳度測試',
    description: '純 JavaScript 打造的色弱小遊戲：在一片色塊中找出顏色略微不同的那一格，隨關卡提升難度，訓練色彩辨識力。',
    image: import.meta.env.BASE_URL + 'color-test-cover.jpg',
    tags: ['JavaScript', 'HTML', 'CSS'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'color-test.html',
    itinerary: [
      '用 JavaScript 動態產生色塊格子',
      '計算每關的目標色與差異色',
      '加入計分與關卡難度遞增邏輯',
      '撰寫互動樣式與點擊判定',
    ],
  },
  {
    id: 7,
    title: '會員資料小卡',
    description: '以 Vue 3 打造的會員資料卡：表單輸入透過雙向綁定即時更新，動態預覽概念藝術風格的個人化會員卡片。',
    image: import.meta.env.BASE_URL + 'member-card-cover.jpg',
    tags: ['Vue 3', 'HTML', 'CSS'],
    githubUrl: 'https://github.com/emily100390/portfolios-web',
    demoUrl: import.meta.env.BASE_URL + 'member-card.html',
    itinerary: [
      '設計會員卡的版面與概念藝術風格',
      '用 v-model 實作表單即時雙向綁定',
      '依輸入內容動態渲染卡片預覽',
      '撰寫互動樣式與 RWD 排版',
    ],
  },
  {
    id: 8,
    title: 'Culture 藝文平台文化體驗改版提案',
    description: '強大的聚合搜尋為核心、個人化推薦為輔，讓找藝文活動從此又快又準的團體改版提案。',
    image: import.meta.env.BASE_URL + 'culture-cover.jpg',
    tags: ['團體專案', 'Vue 3', 'Tailwind CSS', 'Pinia'],
    githubUrl: 'https://github.com/Digi-Pack/NCHU-11503-iCulture',
    demoUrl: 'https://nchu-11503-iculture.dev-hub.io/',
    itinerary: [
      '跨類型聚合搜尋引擎：統一索引活動、展覽、工作坊等多維度資料，一次搜尋即可涵蓋全面藝文資訊',
      '優化資訊架構與篩選機制：重設分類體系、簡化選單層級，減少操作路徑並提升瀏覽效率',
      '地圖一站式定位：整合地理位置資料，支援地圖檢視活動分佈，解決跨類型活動查找分散的問題',
      '個人化推薦與視覺升級：基於用戶行為建立推薦引擎，並重構視覺設計強化文化體驗感',
    ],
    contributions: [
      '詳細頁面前端開發：建立活動詳細資訊頁（活動介紹、時間地點、票務資訊等）的整體布局',
      '台灣地圖與縣市主題頁：建立互動台灣地圖，串聯全台各縣市街頭藝人網站，優化地圖導航與跨站連接邏輯',
      '建立頁內所需的 card UI 元件，提升頁面開發效率',
      '圖片資源最佳化與管理：處理活動圖片下載、壓縮，確保頁面加載性能',
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
        <div class="section-label">▎{{ project.contributions ? '專案重點' : '開發流程' }}</div>
        <ol class="list">
          <li v-for="(step, i) in project.itinerary" :key="i">{{ step }}</li>
        </ol>
      </div>

      <div v-if="project.contributions" class="retro-card contrib-card">
        <div class="section-label">▎我在團隊中的貢獻</div>
        <ul class="list">
          <li v-for="(item, i) in project.contributions" :key="i">{{ item }}</li>
        </ul>
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

/* 團隊貢獻區塊：用主題橘做出強調感 */
.contrib-card {
  background: #FFF6EC;
  border-color: #E8883A;
  box-shadow: 3px 3px 0 #E8883A;
}

.contrib-card .section-label {
  color: #C0651C;
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
