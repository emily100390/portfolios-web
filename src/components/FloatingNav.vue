<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const open = ref(false)

const links = [
  { to: '/',         label: '首頁', icon: '🏠' },
  { to: '/skills',   label: '技能', icon: '⚡' },
  { to: '/projects', label: '作品', icon: '🎮' },
  { to: '/resume',   label: '履歷', icon: '📄' },
]

const toggle = () => { open.value = !open.value }
const close = () => { open.value = false }
</script>

<template>
  <div class="fab-root">
    <!-- 點空白處收合 -->
    <div v-if="open" class="fab-backdrop" @click="close"></div>

    <!-- 展開的導覽選項 -->
    <Transition name="fab-stagger">
      <ul v-if="open" class="fab-menu">
        <li v-for="(link, i) in links" :key="link.to" :style="{ '--i': i }">
          <RouterLink :to="link.to" class="fab-item" @click="close">
            <span class="fab-item-label">{{ link.label }}</span>
            <span class="fab-item-icon">{{ link.icon }}</span>
          </RouterLink>
        </li>
      </ul>
    </Transition>

    <!-- 主按鈕 -->
    <button
      class="fab-main"
      :class="{ open }"
      @click="toggle"
      :aria-label="open ? '收合導覽選單' : '開啟導覽選單'"
      :aria-expanded="open"
    >
      <span class="fab-plus">＋</span>
    </button>
  </div>
</template>

<style scoped>
.fab-root {
  position: fixed;
  /* 桌機：固定在「內容右側的留白區」，不覆蓋內容。
     以全站最寬內容 1180px 為基準，算出右側 gutter，
     再讓按鈕貼在內容右緣外側；留白不足時用 max() 退回貼右邊緣。 */
  top: 80%;
  transform: translateY(10%);
  right: max(1rem, calc((100% - 1180px) / 2 - 64px));
  z-index: 200;
}

/* 點擊空白處收合用的透明遮罩 */
.fab-backdrop {
  position: fixed;
  inset: 0;
  z-index: -1;
}

/* 主按鈕：咖啡色圓鈕 */
.fab-main {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: #6F4E37;
  border: 2.5px solid #2A1F0E;
  box-shadow: 3px 3px 0 #2A1F0E;
  color: #FAF3E0;
  font-size: 1.9rem;
  line-height: 1;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.15s ease, box-shadow 0.15s ease, background 0.15s ease;
}

.fab-main:hover { background: #5A3F2C; }

.fab-main:active {
  transform: translate(3px, 3px);
  box-shadow: none;
}

.fab-plus {
  transition: transform 0.25s ease;
  margin-top: -2px;
}

.fab-main.open .fab-plus { transform: rotate(135deg); }

/* 展開選單 */
.fab-menu {
  list-style: none;
  margin: 0;
  padding: 0;
  position: absolute;
  right: 4px;
  bottom: 68px;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.6rem;
}

.fab-item {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 999px;
  padding: 0.3rem 0.4rem 0.3rem 0.85rem;
  box-shadow: 2px 2px 0 #2A1F0E;
  transition: transform 0.12s ease, background 0.12s ease;
}

.fab-item:hover { transform: translateX(-3px); }

.fab-item-label {
  font-size: 0.85rem;
  font-weight: 700;
  color: #2A1F0E;
  white-space: nowrap;
}

.fab-item-icon {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: #E8883A;
  border: 1.5px solid #2A1F0E;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.95rem;
  flex-shrink: 0;
}

/* 目前所在頁高亮 */
.fab-item.router-link-exact-active {
  background: #2A1F0E;
}
.fab-item.router-link-exact-active .fab-item-label { color: #FAF3E0; }

/* 逐項展開動畫 */
.fab-stagger-enter-active li,
.fab-stagger-leave-active li {
  transition: opacity 0.2s ease, transform 0.2s ease;
  transition-delay: calc(var(--i) * 0.05s);
}
.fab-stagger-enter-from li {
  opacity: 0;
  transform: translateY(10px) scale(0.9);
}
.fab-stagger-leave-to li {
  opacity: 0;
  transform: translateY(10px) scale(0.9);
}

/* 平板 / 手機：改回右下角，但往上抬約 2 顆按鈕的高度 */
@media (max-width: 768px) {
  .fab-root {
    top: auto;
    transform: none;
    right: 1rem;
    bottom: 7rem;
  }
}

@media (max-width: 480px) {
  .fab-root { right: 0.9rem; bottom: 6.5rem; }
  .fab-main { width: 50px; height: 50px; font-size: 1.7rem; }
  .fab-menu { bottom: 62px; }
}
</style>
