<script setup>
import { ref, onMounted } from 'vue'

const uuid = ref('')
const history = ref([]) // Lưu lại lịch sử 5 mã gần nhất

const generateUUID = () => {
  // crypto.randomUUID() là hàm chuẩn của trình duyệt hiện đại
  const newId = crypto.randomUUID()
  uuid.value = newId
  
  // Thêm vào đầu lịch sử
  history.value.unshift(newId)
  // Chỉ giữ lại 5 cái gần nhất
  if (history.value.length > 5) history.value.pop()
}

const copyToClipboard = (text) => {
  navigator.clipboard.writeText(text)
  // Hiệu ứng visual đơn giản: đổi chữ nút bấm (bạn có thể phát triển thêm)
  alert(`Đã copy: ${text}`)
}

// Tạo ngay 1 cái khi vừa vào
onMounted(() => {
  generateUUID()
})
</script>

<template>
  <div class="tool-container">
    <h3>UUID / GUID Generator</h3>
    
    <div class="main-box">
      <div class="uuid-display">{{ uuid }}</div>
      
      <div class="action-row">
        <button class="action-btn generate-btn" @click="generateUUID">
          🔄 Tạo mới (Regenerate)
        </button>
        <button class="action-btn copy-btn" @click="copyToClipboard(uuid)">
          📋 Copy
        </button>
      </div>
    </div>

    <div v-if="history.length > 1" class="history-box">
      <h4>Lịch sử gần đây:</h4>
      <ul>
        <li v-for="(id, index) in history" :key="index">
          <span>{{ id }}</span>
          <button class="mini-copy" @click="copyToClipboard(id)">Copy</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.main-box {
  text-align: center;
  padding: 30px;
  background: #f8fafc;
  border: 2px dashed #cbd5e1;
  border-radius: 10px;
  margin-bottom: 20px;
}

.uuid-display {
  font-family: monospace;
  font-size: 2em;
  font-weight: bold;
  color: #2563eb;
  word-break: break-all;
  margin-bottom: 20px;
}

.action-row {
  display: flex;
  justify-content: center;
  gap: 15px;
}

.action-btn {
  padding: 10px 20px;
  font-size: 1em;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  transition: transform 0.1s;
}
.action-btn:active { transform: translateY(2px); }

.generate-btn { background-color: #2563eb; }
.generate-btn:hover { background-color: #1d4ed8; }

.copy-btn { background-color: #10b981; }
.copy-btn:hover { background-color: #059669; }

.history-box h4 { margin-top: 0; color: #64748b; }
.history-box ul { list-style: none; padding: 0; }
.history-box li {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid #eee;
  font-family: monospace;
  color: #475569;
}
.mini-copy {
  border: 1px solid #cbd5e1;
  background: white;
  cursor: pointer;
  border-radius: 4px;
  font-size: 0.8em;
}
.mini-copy:hover { background: #f1f5f9; }
</style>