<script setup>
import NavBar from './components/NavBar.vue'
import FloatingNav from './components/FloatingNav.vue'
</script>

<template>
  <NavBar />
  <RouterView />
  <FloatingNav />
</template>

<style>
*, *::before, *::after { box-sizing: border-box; }

html {
  -webkit-text-size-adjust: 100%;
}

body {
  margin: 0;
  background-color: #F2E6C9;
  font-family: 'Microsoft JhengHei', 'PingFang TC', 'Noto Sans TC', sans-serif;
  color: #2A1F0E;
  font-size: 16px;   /* 確立桌機基準字級（1rem = 16px）*/
  line-height: 1.6;  /* 內文預設行高，提升可讀性 */
  overflow-x: hidden; /* 安全網：避免任何元件溢出造成橫向捲動 */
}

img {
  max-width: 100%; /* 圖片預設不超出容器 */
}
</style>
