<script setup>
// ✅ 引入 LoginForm.vue 组件（原 JSON.vue 暂时注释）
import LoginForm from './components/LoginForm.vue'
// import JSON from './components/JSON.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <h1 class="app-title">YWANG Library</h1>
  </header>

  <main>
    <!-- ✅ 加载新的 LoginForm 组件 -->
    <LoginForm />

    <!-- 原 JSON 组件已注释 -->
    <!-- <JSON /> -->
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
