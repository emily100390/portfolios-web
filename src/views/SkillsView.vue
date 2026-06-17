<script setup>
import SkillCard from '../components/SkillCard.vue'

const skills = [
  { name: 'HTML',       iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/html5/html5-original.svg',           level: 5, cardColor: 'teal' },
  { name: 'CSS',        iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/css3/css3-original.svg',             level: 4, cardColor: 'teal' },
  { name: 'JavaScript', iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/javascript/javascript-original.svg', level: 4, cardColor: 'coral' },
  { name: 'Vue 3',      iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg',           level: 4, cardColor: 'teal' },
  { name: 'Git',        iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg',               level: 3, cardColor: 'coral' },
  { name: 'RWD',        iconImg: 'https://api.iconify.design/mdi:monitor-cellphone.svg?color=%232A1F0E',                          level: 4, cardColor: 'grey' },
]

const lockedSkills = [
  { name: 'React',      iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original.svg' },
  { name: 'TypeScript', iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg' },
  { name: 'Node.js',    iconImg: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original.svg' },
]
</script>

<template>
  <div class="skills-view">
    <div class="section-header">
      <h2 class="section-title">我的技能 <span>MY SKILLS</span></h2>
    </div>
    <div class="skills-grid">
      <SkillCard
        v-for="skill in skills"
        :key="skill.name"
        v-bind="skill"
      />
    </div>

    <div class="section-header locked-header">
      <h2 class="section-title locked-title">未解鎖技能 <span>LOCKED</span></h2>
    </div>
    <div class="skills-grid">
      <div v-for="locked in lockedSkills" :key="locked.name" class="locked-card">
        <span class="lock-icon">🔒</span>
        <img :src="locked.iconImg" :alt="locked.name" class="locked-img" />
        <div class="locked-name">{{ locked.name }}</div>
        <div class="locked-stars">
          <span v-for="n in 5" :key="n">★</span>
        </div>
        <div class="unlock-tip">請課金解鎖</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.skills-view {
  max-width: 680px;
  margin: 0 auto 2rem;
  padding: 0 1rem;
}

.section-header {
  border-bottom: 3px solid #2A1F0E;
  margin-bottom: 1.25rem;
  padding-bottom: 0.35rem;
}

.locked-header {
  margin-top: 2rem;
  border-bottom-style: dashed;
  border-bottom-color: #6b4e2a;
}

.section-title {
  margin: 0;
  font-size: 1.45rem;
  font-weight: 900;
}

.section-title span {
  font-size: 0.95rem;
  font-weight: 500;
  opacity: 0.65;
  margin-left: 0.4rem;
}

.locked-title {
  color: #6b4e2a;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.75rem;
}

/* ── locked card ── */
.locked-card {
  position: relative;
  background: #614c43;
  border: 2px dashed #4a3020;
  border-radius: 10px;
  padding: 1rem 0.75rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
  box-shadow: 3px 3px 0 #4a3020;
  cursor: not-allowed;
  background-image: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 3px,
    rgba(0,0,0,0.18) 3px,
    rgba(0,0,0,0.18) 4px
  );
}

.lock-icon {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -55%);
  font-size: 1.6rem;
  z-index: 2;
  filter: drop-shadow(0 0 4px #000);
}

.locked-img {
  width: 47px;
  height: 47px;
  object-fit: contain;
  filter: grayscale(1) brightness(0.25);
}

.locked-name {
  font-weight: 700;
  font-size: 0.88rem;
  color: #4a3a2a;
}

.locked-stars span {
  font-size: 0.9rem;
  color: #2e2318;
}

/* ── tooltip ── */
.unlock-tip {
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%);
  background: #2A1F0E;
  color: #FAF3E0;
  font-size: 0.72rem;
  font-weight: 700;
  padding: 0.28rem 0.65rem;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  transition: opacity 0.15s;
  pointer-events: none;
  z-index: 10;
}

.unlock-tip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 5px solid transparent;
  border-top-color: #2A1F0E;
}

.locked-card:hover .unlock-tip {
  opacity: 1;
}
</style>
