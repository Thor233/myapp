<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-card">
      <div class="modal-header">
        <h2>添加新应用</h2>
        <button class="modal-close" @click="$emit('close')">&times;</button>
      </div>

      <form class="modal-body" @submit.prevent="handleSubmit">
        <label>
          <span>应用名称 *</span>
          <input
            v-model="form.name"
            type="text"
            placeholder="例如：GitHub"
            maxlength="20"
            required
          />
        </label>

        <label>
          <span>网址 *</span>
          <input
            v-model="form.url"
            type="url"
            placeholder="https://..."
            required
          />
        </label>

        <label>
          <span>图标地址</span>
          <div class="icon-row">
            <input
              v-model="form.icon"
              type="url"
              placeholder="https://... 留空则自动获取"
            />
            <div class="icon-preview" v-if="form.icon">
              <img :src="form.icon" @error="iconBad = true" v-if="!iconBad" />
              <span v-else>📦</span>
            </div>
          </div>
        </label>

        <label>
          <span>描述</span>
          <input
            v-model="form.description"
            type="text"
            placeholder="简短描述（选填）"
            maxlength="30"
          />
        </label>

        <div class="modal-actions">
          <button type="button" class="btn-cancel" @click="$emit('close')">取消</button>
          <button type="submit" class="btn-save">添加</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['close', 'add'])

const form = reactive({
  name: '',
  url: '',
  icon: '',
  description: '',
})

const iconBad = ref(false)

function handleSubmit() {
  if (!form.name.trim() || !form.url.trim()) return
  emit('add', {
    name: form.name.trim(),
    url: form.url.trim(),
    icon: form.icon.trim(),
    description: form.description.trim(),
  })
  form.name = ''
  form.url = ''
  form.icon = ''
  form.description = ''
  iconBad.value = false
  emit('close')
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 20px;
}

.modal-card {
  background: #fff;
  border-radius: 16px;
  width: 100%;
  max-width: 420px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
  animation: modalIn 0.2s ease;
}

@keyframes modalIn {
  from { opacity: 0; transform: translateY(20px) scale(0.96); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 24px 0;
}

.modal-header h2 {
  font-size: 18px;
  font-weight: 600;
}

.modal-close {
  width: 32px;
  height: 32px;
  border: none;
  background: transparent;
  font-size: 22px;
  color: #999;
  cursor: pointer;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.15s, color 0.15s;
}

.modal-close:hover { background: #f0f0f0; color: #333; }

.modal-body {
  padding: 20px 24px 24px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.modal-body label {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.modal-body label > span {
  font-size: 13px;
  font-weight: 500;
  color: #555;
}

.modal-body input {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 10px;
  font-size: 14px;
  outline: none;
  transition: border-color 0.2s;
  box-sizing: border-box;
}

.modal-body input:focus {
  border-color: var(--primary);
}

.icon-row {
  display: flex;
  gap: 10px;
  align-items: center;
}

.icon-row input {
  flex: 1;
}

.icon-preview {
  width: 38px;
  height: 38px;
  border-radius: 10px;
  overflow: hidden;
  background: #f8f8fc;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.icon-preview img {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

.modal-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 8px;
}

.btn-cancel, .btn-save {
  padding: 10px 24px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s, opacity 0.2s;
  border: none;
}

.btn-cancel {
  background: #f0f0f0;
  color: #555;
}
.btn-cancel:hover { background: #e0e0e0; }

.btn-save {
  background: var(--primary);
  color: #fff;
}
.btn-save:hover { opacity: 0.88; }
</style>
