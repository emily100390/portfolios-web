<script setup>
defineProps({
  id:          { type: Number,  required: true },
  title:       { type: String,  required: true },
  description: { type: String,  required: true },
  image:       { type: String,  required: true },
  tags:        { type: Array,   default: () => [] },
  githubUrl:   { type: String,  default: '#' },
  demoUrl:     { type: String,  default: '#' },
  isFavorited: { type: Boolean, default: false },
  level:       { type: Number,  default: 4 },
})

const emit = defineEmits(['view-detail', 'toggle-favorite'])
</script>

<template>
  <div class="project-card">
    <img :src="image" :alt="title" class="card-img" />
    <div class="card-left">
      <h3 class="card-title">{{ title }}</h3>
      <p class="card-desc">{{ description }}</p>
      <div class="stars">
        <span v-for="n in 5" :key="n" class="star" :class="{ filled: n <= level }">★</span>
      </div>
    </div>
    <div class="card-right">
      <button type="button" class="btn-view" @click="emit('view-detail', id)">瀏覽作品</button>
      <button type="button" class="btn-fav"  @click="emit('toggle-favorite', id)">
        {{ isFavorited ? '❤️' : '🤍' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.project-card {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 10px;
  padding: 1.2rem 1.5rem;
  display: flex;
  align-items: center;
  gap: 1.2rem;
  box-shadow: 3px 3px 0 #2A1F0E;
}

.card-img {
  width: 144px;
  height: 104px;
  object-fit: cover;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  flex-shrink: 0;
}

.card-left { flex: 1; min-width: 0; }

.card-title {
  margin: 0 0 0.36rem;
  font-size: 1.25rem;
  font-weight: 700;
}

.card-desc {
  margin: 0 0 0.6rem;
  color: #5A4A30;
  font-size: 1.05rem;
  line-height: 1.5;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.stars { display: flex; gap: 2px; }

.star       { font-size: 1.05rem; color: #D4C5A0; }
.star.filled { color: #F5C518; }

.card-right {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  flex-shrink: 0;
}

.btn-view {
  background: #FAF3E0;
  border: 2px solid #2A1F0E;
  border-radius: 6px;
  padding: 0.46rem 1rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 1.05rem;
  font-family: inherit;
  box-shadow: 2px 2px 0 #2A1F0E;
  white-space: nowrap;
  transition: all 0.1s;
}

.btn-view:hover {
  background: #2A1F0E;
  color: #FAF3E0;
  box-shadow: none;
  transform: translate(2px, 2px);
}

.btn-fav {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.3rem;
  line-height: 1;
}

/* ── RWD：手機 ── */
@media (max-width: 480px) {
  .project-card {
    padding: 0.85rem;
    gap: 0.75rem;
  }
  .card-img {
    width: 72px;
    height: 56px;
  }
  .card-title { font-size: 1rem; }
  .card-desc { font-size: 0.85rem; }
  .btn-view {
    padding: 0.35rem 0.6rem;
    font-size: 0.78rem;
  }
}

@media (max-width: 360px) {
  .card-img { display: none; } /* 極窄螢幕隱藏縮圖，留空間給文字 */
}
</style>
