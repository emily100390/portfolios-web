<script setup>
import ProfileCard from '../components/ProfileCard.vue'
import CharacterSection from '../components/CharacterSection.vue'

const baseUrl = import.meta.env.BASE_URL
</script>

<template>
  <ProfileCard
    :avatar="`${baseUrl}avatar.jpg`"
    name="蔡秀甄"
    title="前端工程師 / 小說 漫畫 愛好者"
    bio="熱愛打造流暢的使用者體驗，專注於 Vue 3、RWD 與無障礙設計。目前正在尋找新機會！"
  />
  <CharacterSection />
</template>
