<script setup>
import { ref } from 'vue'

const input = ref('')
const output = ref('')
const isError = ref(false)

// Hàm Mã hóa (Encode): Text -> Base64
const handleEncode = () => {
  try {
    // btoa là hàm có sẵn của trình duyệt để mã hóa
    output.value = btoa(input.value)
    isError.value = false
  } catch (e) {
    output.value = 'Lỗi: Không thể mã hóa chuỗi này (có thể do ký tự đặc biệt).'
    isError.value = true
  }
}

// Hàm Giải mã (Decode): Base64 -> Text
const handleDecode = () => {
  try {
    // atob là hàm có sẵn để giải mã
    output.value = atob(input.value)
    isError.value = false
  } catch (e) {
    output.value = 'Lỗi: Chuỗi Base64 không hợp lệ.'
    isError.value = true
  }
}

// Hàm xóa
const clearAll = () => {
  input.value = ''
  output.value = ''
  isError.value = false
}
</script>

<template>
  <div class="tool-container">
    <h3>Base64 Encode / Decode</h3>
    
    <textarea 
      v-model="input" 
      placeholder="Nhập văn bản hoặc chuỗi Base64..." 
    ></textarea>
    
    <div class="btn-group">
      <button class="action-btn" @click="handleEncode">Encode (Mã hóa)</button>
      <button class="action-btn" style="background-color: #0ea5e9;" @click="handleDecode">Decode (Giải mã)</button>
      <button class="clear-btn" @click="clearAll">Clear</button>
    </div>
    
    <div class="result-box">
      <strong>Kết quả:</strong>
      <p :class="{ error: isError }" style="word-break: break-all;">
        {{ output }}
      </p>
    </div>
  </div>
</template>