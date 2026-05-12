<template>
  <a :href="app.url" target="_blank" class="app-card" rel="noopener">
    <div class="app-icon">
      <img :src="app.icon" :alt="app.name" @error="onImgError" />
    </div>
    <span class="app-name">{{ app.name }}</span>
  </a>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  app: { type: Object, required: true },
})

const fallbackIcon =
  'data:image/svg+xml,' +
  encodeURIComponent(
    '<svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 48 48"><rect width="48" height="48" rx="10" fill="#e0e0e0"/><text x="24" y="30" text-anchor="middle" font-size="20">📦</text></svg>'
  )

function onImgError(e) {
  e.target.src = fallbackIcon
}
</script>

<style scoped>
.app-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 20px 12px;
  background: var(--card-bg);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  text-decoration: none;
  color: var(--text);
  transition: transform 0.2s, box-shadow 0.2s;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

.app-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-hover);
}

.app-card:active {
  transform: scale(0.96);
}

.app-icon {
  width: 56px;
  height: 56px;
  border-radius: 14px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8f8fc;
}

.app-icon img {
  width: 48px;
  height: 48px;
  object-fit: contain;
}

.app-name {
  font-size: 13px;
  font-weight: 500;
  text-align: center;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: 100%;
}
</style>
