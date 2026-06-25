<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()
const pdfUrl = import.meta.env.BASE_URL + 'resume.pdf'
</script>

<template>
  <div class="resume-view">
    <div class="resume-bar">
      <button type="button" class="btn-back" @click="router.push('/')">← 回到首頁</button>
      <a :href="pdfUrl" target="_blank" rel="noopener" class="btn-download">⬇ 下載 / 新分頁開啟</a>
    </div>

    <h1 class="resume-title">我的履歷</h1>

    <div class="pdf-frame">
      <iframe :src="pdfUrl" title="蔡秀甄履歷" class="pdf-iframe"></iframe>
    </div>

    <p class="fallback">
      若無法正常顯示，請
      <a :href="pdfUrl" target="_blank" rel="noopener">點此於新分頁開啟 PDF</a>。
    </p>
  </div>
</template>

<style scoped>
.resume-view {
  max-width: 900px;
  margin: 1.5rem auto 3rem;
  padding: 0 1rem;
}

.resume-bar {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  align-items: center;
  margin-bottom: 1.25rem;
}

.btn-back,
.btn-download {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.38rem 0.9rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.9rem;
  font-family: inherit;
  text-decoration: none;
  color: #2A1F0E;
  box-shadow: 2px 2px 0 #2A1F0E;
  transition: all 0.1s;
}

.btn-back:hover,
.btn-download:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

.resume-title {
  font-size: 1.55rem;
  font-weight: 900;
  margin: 0 0 1rem;
}

.pdf-frame {
  border: 2.5px solid #2A1F0E;
  border-radius: 10px;
  box-shadow: 4px 4px 0 #2A1F0E;
  overflow: hidden;
  background: #fff;
}

.pdf-iframe {
  display: block;
  width: 100%;
  height: 80vh;
  min-height: 480px;
  border: 0;
}

.fallback {
  margin-top: 1rem;
  font-size: 0.95rem;
  color: #5A4A30;
}

.fallback a {
  color: #E8883A;
  font-weight: 700;
}

/* ── RWD：手機 ── */
@media (max-width: 480px) {
  .resume-title { font-size: 1.25rem; }
  .pdf-iframe { height: 70vh; }
}
</style>
