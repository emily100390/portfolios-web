<script setup>
defineProps({
  avatar: { type: String, required: true },
  name:   { type: String, required: true },
  title:  { type: String, required: true },
  bio:    { type: String, required: true },
})
</script>

<template>
  <div class="profile-wrapper">
    <div class="profile-card">
      <div class="profile-left">
        <h1 class="greeting">Hi，我是{{ name }}</h1>
        <ul class="tag-list">
          <li>「{{ title }}」</li>
          <li>「{{ bio }}」</li>
        </ul>
        <RouterLink to="/resume" class="btn-more">了解更多 ▶</RouterLink>
      </div>
      <div class="profile-right">
        <img :src="avatar" :alt="name" class="avatar" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.profile-wrapper {
  max-width: 680px;
  margin: 1.75rem auto;
  padding: 0 1rem;
}

.profile-card {
  background: #FAF3E0;
  border: 2.5px solid #2A1F0E;
  border-radius: 10px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column-reverse;
  align-items: center;
  gap: 1.25rem;
  box-shadow: 5px 5px 0 #2A1F0E;
}

.profile-left {
  flex: 1;
  text-align: center;
}

.greeting {
  font-size: 1.55rem;
  font-weight: 900;
  margin: 0 0 0.9rem;
}

.tag-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.tag-list li {
  font-size: 1rem;
  font-weight: 500;
  line-height: 1.6;
}

.btn-more {
  display: inline-block;
  margin-top: 1.1rem;
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.42rem 1.2rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.92rem;
  font-family: inherit;
  text-decoration: none;
  color: #2A1F0E;
  box-shadow: 2px 2px 0 #2A1F0E;
  transition: all 0.1s;
}

.btn-more:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

.avatar {
  width: 110px;
  height: 140px;
  object-fit: cover;
  border: 2.5px solid #2A1F0E;
  border-radius: 6px;
  box-shadow: 3px 3px 0 #2A1F0E;
  flex-shrink: 0;
}

/* ── RWD：平板以下改回大頭照在右、敘述在左的橫向排版 ── */
@media (max-width: 900px) {
  .profile-card {
    flex-direction: row;
  }
  .profile-left { text-align: left; }
}

/* ── RWD：手機 ── */
@media (max-width: 480px) {
  .profile-card {
    padding: 1.1rem;
    gap: 1rem;
    box-shadow: 4px 4px 0 #2A1F0E;
  }
  .greeting {
    font-size: 1.25rem;
    margin-bottom: 0.7rem;
  }
  .tag-list li { font-size: 0.92rem; }
  .avatar {
    width: 90px;
    height: 116px;
  }
}

@media (max-width: 360px) {
  .greeting { font-size: 1.1rem; }
  .avatar { width: 78px; height: 100px; }
}
</style>
