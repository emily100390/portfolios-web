<script setup>
import ProfileCard from '../components/ProfileCard.vue'
import CharacterSection from '../components/CharacterSection.vue'
import FeaturedCarousel from '../components/FeaturedCarousel.vue'

const baseUrl = import.meta.env.BASE_URL
</script>

<template>
  <div class="home-layout">
    <div class="home-left">
      <ProfileCard
        :avatar="`${baseUrl}avatar.jpg`"
        name="蔡秀甄"
        title="前端工程師 / 小說 漫畫 愛好者"
        bio="🤖 熱愛 AI、機器人與動漫小說 | 🔮 相信未來是次世代顯示與投影技術的世界。🪙端無限👍"
      />
    </div>
    <div class="home-right">
      <CharacterSection />
    </div>
  </div>

  <FeaturedCarousel />
</template>

<style scoped>
.home-layout {
  max-width: 1180px;
  margin: 0 auto;
  padding: 1.5rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.home-left {
  flex: 1 1 0;
  min-width: 0;
}

.home-right {
  flex: 1 1 0;
  min-width: 0;
}

/* 桌機版讓兩欄各自填滿欄寬，移除元件原本的 max-width 限制 */
.home-left :deep(.profile-wrapper),
.home-right :deep(.char-section) {
  max-width: 100%;
  margin: 0 auto;
}

/* ── RWD：平板以下改回上下堆疊 ── */
@media (max-width: 900px) {
  .home-layout {
    flex-direction: column;
    gap: 1;
  }
  .home-left,
  .home-right {
    width: 100%;
  }
  .home-left :deep(.profile-wrapper),
  .home-right :deep(.char-section) {
    max-width: 680px;
  }
}
</style>
