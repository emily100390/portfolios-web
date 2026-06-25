<script setup>
import { ref, computed, nextTick } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const baseUrl = import.meta.env.BASE_URL

const items = [
  {
    id: 4,
    title: '神奇生物圖鑑',
    img: 'biology-cover.jpg',
    desc: '報紙剪報風格的主題收藏館，可填寫調查表單、星級評分、即時統計與分類篩選，並用 Web Audio API 模擬蓋章音效。',
  },
  {
    id: 8,
    title: 'Culture 藝文平台改版提案',
    img: 'culture-cover.jpg',
    desc: '從翻找藝文活動資料，到關鍵字一搜即中。以聚合搜尋為核心、個人化推薦為輔的團體改版提案。',
  },
  {
    id: 5,
    title: '城市旅遊導覽',
    img: 'city-travel-cover.jpg',
    desc: '以 Vue Router 打造的多頁式旅遊導覽：城市列表、各城市景點、景點詳細頁，並用 Pinia 管理狀態。',
  },
  {
    id: 3,
    title: '天氣查詢儀表板',
    img: 'weather-cover.jpg',
    desc: '串接 Open-Meteo API，依城市名稱即時顯示目前天氣與五日預報，支援攝氏 / 華氏切換。',
  },
  {
    id: 2,
    title: '任務管理 App',
    img: 'todo-cover.jpg',
    desc: '具備新增、刪除、拖曳排序與本地儲存功能的 Todo 應用程式，資料持久化於 localStorage。',
  },
  {
    id: 6,
    title: '色彩敏銳度測試',
    img: 'color-test-cover.jpg',
    desc: '純 JavaScript 打造的色弱小遊戲：在色塊中找出顏色不同的那一格，難度逐關提升，訓練色彩辨識力。',
  },
  {
    id: 7,
    title: '會員資料小卡',
    img: 'member-card-cover.jpg',
    desc: '以 Vue 3 打造的會員資料卡，輸入即時雙向綁定，動態預覽概念藝術風格的個人化會員卡片。',
  },
]

const active = ref(0)
const activeItem = computed(() => items[active.value])

// 大跳轉時暫時關閉過渡，瞬間就位後再開啟
const noAnim = ref(false)

// 計算每張卡片相對於中央的偏移（含循環，取最近距離）
function offsetOf(i) {
  const n = items.length
  let diff = i - active.value
  if (diff > n / 2) diff -= n
  if (diff < -n / 2) diff += n
  return diff
}

// 依偏移給出 coverflow 的位移、縮放、旋轉、透明度與層級
function slideStyle(i) {
  const off = offsetOf(i)
  const abs = Math.abs(off)
  // 大跳轉時整批關閉過渡，避免卡片橫掃整個舞台
  const transition = noAnim.value
    ? 'none'
    : 'transform 0.6s cubic-bezier(0.25, 0.1, 0.25, 1), opacity 0.6s ease'
  // 超過兩格：滑出舞台外淡出（單位與可見卡同為 %，過渡才能平滑插值、不殘影）
  if (abs > 2) {
    return {
      transform: `translate(-50%, 0) translateX(${off > 0 ? 155 : -155}%) scale(0.7) rotateY(${off > 0 ? -28 : 28}deg) translateZ(${-abs * 120}px)`,
      opacity: 0,
      zIndex: 0,
      pointerEvents: 'none',
      transition,
    }
  }
  // 中央 100% / 兩側遞減 10%，旋轉 12°、透明度遞減 0.3，帶一點立體感
  return {
    transform: `translate(-50%, 0) translateX(${off * 54}%) scale(${1 - abs * 0.1}) rotateY(${off * -12}deg) translateZ(${-abs * 120}px)`,
    opacity: off === 0 ? 1 : 1 - abs * 0.45,
    zIndex: 10 - abs,
    transition,
  }
}

// 切到指定卡片：相鄰一格正常過渡；跨多格則瞬間就位避免橫掃
function goTo(i) {
  const n = items.length
  let diff = i - active.value
  if (diff > n / 2) diff -= n
  if (diff < -n / 2) diff += n
  if (Math.abs(diff) <= 1) {
    active.value = i
    return
  }
  noAnim.value = true
  active.value = i
  nextTick(() => {
    requestAnimationFrame(() => {
      noAnim.value = false
    })
  })
}

function prev() {
  goTo((active.value - 1 + items.length) % items.length)
}
function next() {
  goTo((active.value + 1) % items.length)
}

// 點卡片：中央 → 進詳細頁；側邊 → 切換到該作品
function onCardClick(i) {
  if (i === active.value) {
    router.push('/projects/' + items[i].id)
  } else {
    goTo(i)
  }
}
</script>

<template>
  <section class="featured">
    <h2 class="featured-header">— 精選作品集 —</h2>

    <div class="carousel">
      <button class="arrow arrow-left" @click="prev" aria-label="上一個作品">❮</button>

      <div class="stage">
        <div
          v-for="(item, i) in items"
          :key="item.id"
          class="slide"
          :style="slideStyle(i)"
          @click="onCardClick(i)"
        >
          <div class="frame">
            <img :src="baseUrl + item.img" :alt="item.title" class="frame-img" />
          </div>
        </div>
      </div>

      <button class="arrow arrow-right" @click="next" aria-label="下一個作品">❯</button>
    </div>

    <!-- 圓點指示器 -->
    <div class="dots">
      <button
        v-for="(item, i) in items"
        :key="item.id"
        class="dot"
        :class="{ on: i === active }"
        @click="goTo(i)"
        :aria-label="`切換到 ${item.title}`"
      ></button>
    </div>

    <!-- 當前作品標題 -->
    <h3 class="active-title">{{ activeItem.title }}</h3>

    <!-- 作品介紹面板 -->
    <div class="summary">
      <div class="summary-label">作品介紹</div>
      <p class="summary-desc">{{ activeItem.desc }}</p>
      <RouterLink :to="'/projects/' + activeItem.id" class="summary-link">查看作品 ▶</RouterLink>
    </div>

  </section>
</template>

<style scoped>
.featured {
  max-width: 1080px;
  margin: 1rem auto 3rem;
  padding: 0 1rem;
  text-align: center;
}

.featured-header {
  font-size: 1.9rem;
  font-weight: 900;
  letter-spacing: 0.12em;
  color: #5A4A30;
  margin: 0 0 1.5rem;
}

/* ── 輪播舞台 ── */
.carousel {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  
}

.stage {
  position: relative;
  width: 91%;
  height: 384px;
  /* overflow: hidden; */
  perspective: 1200px;
  transform-style: preserve-3d;
}

.slide {
  position: absolute;
  top: 0;
  left: 50%;
  width: 442px;
  cursor: pointer;
  transform-style: preserve-3d;
  backface-visibility: hidden;
  transition: transform 0.6s cubic-bezier(0.25, 0.1, 0.25, 1), opacity 0.6s ease;
  will-change: transform, opacity;
}

.frame {
  background: #FAF3E0;
  border: 3px solid #2A1F0E;
  border-radius: 16px;
  box-shadow: 7px 7px 0 #2A1F0E;
  padding: 17px;
}

.frame-img {
  display: block;
  width: 100%;
  height: 294px;
  object-fit: cover;
  border: 2px solid #2A1F0E;
  border-radius: 8px;
}

/* ── 箭頭 ── */
.arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 5;
  width: 64px;
  height: 64px;
  border-radius: 50%;
  background: #FAF3E0;
  border: 2.5px solid #2A1F0E;
  box-shadow: 3px 3px 0 #2A1F0E;
  cursor: pointer;
  font-size: 1.6rem;
  font-weight: 900;
  line-height: 1;
  color: #2A1F0E;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.1s;
}

.arrow:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translateY(-50%) translate(3px, 3px);
}

.arrow-left { left: 0; }
.arrow-right { right: 0; }

/* ── 當前標題 ── */
.active-title {
  font-size: 1.95rem;
  font-weight: 900;
  margin: 1.5rem 0 1rem;
}

/* ── 介紹面板 ── */
.summary {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 10px;
  padding: 1.6rem 2.1rem;
  box-shadow: 4px 4px 0 #2A1F0E;
  text-align: left;
}

.summary-label {
  font-weight: 900;
  font-size: 1.35rem;
  color: #E8883A;
  margin-bottom: 0.5rem;
}

.summary-desc {
  margin: 0 0 0.9rem;
  font-size: 1.4rem;
  line-height: 1.65;
  color: #3D2B1F;
}

.summary-link {
  display: inline-block;
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.59rem 1.6rem;
  font-weight: 700;
  font-size: 1.3rem;
  text-decoration: none;
  color: #2A1F0E;
  box-shadow: 2px 2px 0 #2A1F0E;
  transition: all 0.1s;
}

.summary-link:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

/* ── 圓點 ── */
.dots {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 0.25rem;
}

.dot {
  width: 11px;
  height: 11px;
  border-radius: 50%;
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  cursor: pointer;
  padding: 0;
  transition: background 0.15s;
}

.dot.on { background: #E8883A; }

/* ── RWD ── */
/* 平板/手機：螢幕窄，側邊卡片會大幅溢出撐爆版面，這裡把舞台裁切住 */
@media (max-width: 768px) {
  .stage { overflow: hidden; }
}

@media (max-width: 560px) {
  .stage { height: 250px; }
  .slide { width: 276px; }
  .frame-img { height: 180px; }
  .arrow {
    width: 44px;
    height: 44px;
    font-size: 1.1rem;
  }
  .active-title { font-size: 1.35rem; }
  .summary-desc { font-size: 1.05rem; }
}
</style>
