<template>
  <div class="skill-card" :class="cardColor">
    <img v-if="iconImg" :src="iconImg" :alt="name" class="icon-img" />
    <span v-else class="icon-text">{{ icon || name.slice(0, 2).toUpperCase() }}</span>
    <div class="skill-name">{{ name }}</div>
    <div class="stars">
      <span v-for="n in 5" :key="n" class="star" :class="{ filled: n <= level }">★</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  name:        { type: String, required: true },
  icon:        { type: String, default: '' },
  iconImg:     { type: String, default: '' },
  level:       { type: Number, required: true },
  cardColor:   { type: String, default: 'teal' },
})
</script>

<style scoped>
.skill-card {
  border: 2px solid #2A1F0E;
  border-radius: 10px;
  padding: 1rem 0.75rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
  box-shadow: 3px 3px 0 #2A1F0E;
}

.skill-card { background: #7BBFB8; }

.icon-img {
  width: 47px;
  height: 47px;
  object-fit: contain;
}

.icon-text {
  font-weight: 900;
  font-size: 1rem;
  color: #2A1F0E;
}

.skill-name {
  font-weight: 700;
  font-size: 0.88rem;
  color: #2A1F0E;
}

.stars { display: flex; gap: 2px; }

.star       { font-size: 0.9rem; color: rgba(42, 31, 14, 0.22); }
.star.filled { color: #F5C518; }
</style>
