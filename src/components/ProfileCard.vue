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
  align-items: center;
  gap: 1.25rem;
  box-shadow: 5px 5px 0 #2A1F0E;
}

.profile-left { flex: 1; }

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
  font-size: 0.92rem;
  font-weight: 500;
  line-height: 1.5;
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
  .tag-list li { font-size: 0.85rem; }
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
