<template>
  <div class="container">
    <header class="header">
      <h1>我的应用中心</h1>
      <p class="subtitle">常用工具 &amp; 网站导航</p>
    </header>

    <main class="app-grid">
      <AppCard v-for="app in appList" :key="app.id" :app="app" />
    </main>

    <footer class="footer">
      <button class="add-btn" @click="showModal = true">+</button>
    </footer>

    <AddAppModal
      v-if="showModal"
      @close="showModal = false"
      @add="addApp"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import AppCard from './components/AppCard.vue'
import AddAppModal from './components/AddAppModal.vue'
import { apps } from './apps.js'

let nextId = apps.length + 1
const appList = ref([...apps])
const showModal = ref(false)

function addApp(app) {
  appList.value.push({
    id: nextId++,
    ...app,
  })
}
</script>
