<script setup>
import { ref } from 'vue'

// 1. Khai báo các biến trạng thái (State)
const inputJson = ref('')      // Chứa text người dùng nhập
const outputJson = ref(null)   // Chứa kết quả sau khi format
const errorMessage = ref('')   // Chứa thông báo lỗi

// 2. Hàm xử lý Format JSON
const formatJson = () => {
  if (!inputJson.value.trim()) return; // Nếu rỗng thì không làm gì

  try {
    // Thử parse chuỗi nhập vào thành Object
    const parsed = JSON.parse(inputJson.value)
    
    // Nếu thành công: Format lại thành chuỗi với thụt đầu dòng 2 spaces
    outputJson.value = JSON.stringify(parsed, null, 2)
    errorMessage.value = '' // Xóa lỗi cũ nếu có
  } catch (err) {
    // Nếu lỗi: Hiển thị lỗi và xóa kết quả cũ
    errorMessage.value = 'JSON không hợp lệ: ' + err.message
    outputJson.value = null
  }
}

// 3. Hàm xóa sạch (Clear)
const clearAll = () => {
  inputJson.value = ''
  outputJson.value = null
  errorMessage.value = ''
}
</script>

<template>
  <div class="tool-container">
    <h3>JSON Formatter & Validator</h3>
    
    <textarea 
      v-model="inputJson" 
      placeholder="Dán JSON của bạn vào đây..." 
    ></textarea>
    
    <div class="btn-group">
      <button class="action-btn" @click="formatJson">Format / Validate</button>
      <button class="clear-btn" @click="clearAll">Clear</button>
    </div>

    <div v-if="errorMessage" class="error">
      {{ errorMessage }}
    </div>

    <div v-if="outputJson" class="result-box">
      <pre>{{ outputJson }}</pre>
    </div>
  </div>
</template>