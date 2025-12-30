<script setup>
import { ref, watch } from 'vue'
import { jwtDecode } from "jwt-decode"

const token = ref('')
const decodedHeader = ref(null)
const decodedPayload = ref(null)
const errorMessage = ref('')

// Hàm xử lý giải mã
const processDecode = (inputToken) => {
  if (!inputToken.trim()) {
    decodedHeader.value = null
    decodedPayload.value = null
    errorMessage.value = ''
    return
  }

  try {
    // 1. Giải mã Header (thêm option { header: true })
    decodedHeader.value = jwtDecode(inputToken, { header: true })
    
    // 2. Giải mã Payload (dữ liệu chính)
    decodedPayload.value = jwtDecode(inputToken)
    
    errorMessage.value = ''
  } catch (err) {
    errorMessage.value = 'Token JWT không hợp lệ hoặc bị lỗi định dạng.'
    decodedHeader.value = null
    decodedPayload.value = null
  }
}

// Tự động chạy hàm giải mã mỗi khi biến token thay đổi
watch(token, (newVal) => {
  processDecode(newVal)
})
</script>

<template>
  <div class="tool-container">
    <h3>JWT Debugger</h3>
    
    <textarea 
      v-model="token" 
      placeholder="Dán mã JWT (ey...) vào đây..." 
      style="height: 100px;"
    ></textarea>
    
    <div v-if="errorMessage" class="error">
      {{ errorMessage }}
    </div>

    <div class="jwt-results">
      
      <div class="result-box">
        <h4>Header</h4>
        <pre v-if="decodedHeader">{{ JSON.stringify(decodedHeader, null, 2) }}</pre>
        <span v-else class="placeholder">Chưa có dữ liệu...</span>
      </div>

      <div class="result-box">
        <h4>Payload</h4>
        <pre v-if="decodedPayload">{{ JSON.stringify(decodedPayload, null, 2) }}</pre>
        <span v-else class="placeholder">Chưa có dữ liệu...</span>
      </div>

    </div>
  </div>
</template>

<style scoped>
/* CSS riêng cho layout 2 cột của JWT */
.jwt-results {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Chia đôi màn hình */
  gap: 20px;
}

@media (max-width: 600px) {
  .jwt-results {
    grid-template-columns: 1fr; /* Trên điện thoại thì xếp chồng lên nhau */
  }
}

.placeholder { color: #999; font-style: italic; }
h4 { margin-top: 0; margin-bottom: 10px; color: #666; border-bottom: 1px solid #eee; padding-bottom: 5px;}
</style>