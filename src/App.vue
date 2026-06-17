<script setup>
import NavBar from './components/NavBar.vue'
</script>

<template>
  <NavBar />
  <RouterView />
</template>

<style>
*, *::before, *::after { box-sizing: border-box; }

body {
  margin: 0;
  background-color: #F2E6C9;
  font-family: 'Microsoft JhengHei', 'PingFang TC', 'Noto Sans TC', sans-serif;
  color: #2A1F0E;
}
</style>
