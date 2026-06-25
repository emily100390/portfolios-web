<script setup>
</script>

<template>
  <header class="site-header">
    <div class="banner-top">
      <span class="banner-title">青壯年★叛逆錄</span>
      <span class="banner-divider">｜</span>
      <span class="banner-sub">人物大剖析</span>
    </div>
    <div class="warning-bar">▲ 中二魂燃燒中 ▲</div>
    <nav class="navbar">
      <RouterLink to="/">首頁</RouterLink>
      <RouterLink to="/skills">技能</RouterLink>
      <RouterLink to="/projects">作品</RouterLink>
      <RouterLink to="/resume">履歷</RouterLink>
    </nav>
  </header>
</template>

<style scoped>
.site-header {
  position: sticky;
  top: 0;
  z-index: 100;
}

.banner-top {
  background: #2A1F0E;
  color: #F2E6C9;
  text-align: center;
  padding: 0.55rem 1rem;
  font-weight: 900;
  font-size: 1.1rem;
  letter-spacing: 0.08em;
}

.banner-divider {
  margin: 0 0.5rem;
  opacity: 0.5;
}

.warning-bar {
  background: #E8883A;
  color: #F2E6C9;
  text-align: center;
  padding: 0.22rem;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.25em;
}

.navbar {
  background: #F2E6C9;
  display: flex;
  justify-content: center;
  border-bottom: 2.5px solid #2A1F0E;
}

.navbar a {
  color: #2A1F0E;
  text-decoration: none;
  font-weight: 700;
  padding: 0.45rem 1.6rem;
  border-right: 1px solid #2A1F0E;
  font-size: 0.95rem;
  transition: background 0.12s;
}

.navbar a:first-child { border-left: 1px solid #2A1F0E; }

.navbar a:hover,
.navbar a.router-link-active {
  background: #2A1F0E;
  color: #F2E6C9;
}

/* ── RWD：手機 ── */
@media (max-width: 480px) {
  .banner-top {
    font-size: 0.95rem;
    padding: 0.5rem 0.5rem;
    letter-spacing: 0.04em;
  }
  .banner-divider { margin: 0 0.3rem; }
  .warning-bar {
    font-size: 0.7rem;
    letter-spacing: 0.15em;
  }
  .navbar a {
    padding: 0.45rem 0;
    flex: 1;
    text-align: center;
    font-size: 0.85rem;
  }
}
</style>
