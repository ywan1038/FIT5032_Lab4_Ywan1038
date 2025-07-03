<script setup>
import JSON from './components/JSON.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <h1 class="app-title">YWANG Library</h1>
  </header>

  <main>
    <!-- 这里加载你的 JSON.vue 组件 -->
    <JSON />
  </main>
</template>

<style scoped>
header {
  text-align: center;
  padding: 2rem;
  background-color: #42b883;
  color: white;
}

.app-title {
  margin-top: 0.5rem;
  font-size: 2rem;
  font-weight: bold;
}

.logo {
  display: block;
  margin: 0 auto;
}

main {
  padding: 2rem;
}
</style>
