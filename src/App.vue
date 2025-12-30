<script setup>
import { ref } from 'vue'
import JsonEditor from './components/JsonEditor.vue'
import Base64Tool from './components/Base64Tool.vue'
import JwtDebugger from './components/JwtDebugger.vue'
import TimestampConverter from './components/TimestampConverter.vue'
import UuidGenerator from './components/UuidGenerator.vue'

const activeTab = ref('json')
const currentYear = new Date().getFullYear() // Tự động lấy năm hiện tại
</script>

<template>
  <div class="container">
    <div class="header">
      <h1>Dev All-In-One Tools</h1>
      <p class="subtitle">Utility toolkit for developers</p>
    </div>
    
    <div class="tabs">
      <button class="tab-btn" :class="{ active: activeTab === 'json' }" @click="activeTab = 'json'">JSON</button>
      <button class="tab-btn" :class="{ active: activeTab === 'base64' }" @click="activeTab = 'base64'">Base64</button>
      <button class="tab-btn" :class="{ active: activeTab === 'jwt' }" @click="activeTab = 'jwt'">JWT</button>
      <button class="tab-btn" :class="{ active: activeTab === 'timestamp' }" @click="activeTab = 'timestamp'">Time</button>
      <button class="tab-btn" :class="{ active: activeTab === 'uuid' }" @click="activeTab = 'uuid'">UUID</button>
    </div>

    <div class="content">
      <div v-if="activeTab === 'json'"><JsonEditor /></div>
      <div v-if="activeTab === 'base64'"><Base64Tool /></div>
      <div v-if="activeTab === 'jwt'"><JwtDebugger /></div>
      <div v-if="activeTab === 'timestamp'"><TimestampConverter /></div>
      <div v-if="activeTab === 'uuid'"><UuidGenerator /></div>
    </div>

    <footer class="app-footer">
      <p>
        Duong Quoc Hao © {{ currentYear }}
      </p>
    </footer>

  </div>
</template>

<style scoped>
/* 1. Sửa class .container để nó cao ít nhất bằng cả màn hình */
.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  
  /* Cấu hình Flexbox để đẩy Footer xuống */
  min-height: 100vh;       /* Chiều cao tối thiểu = 100% màn hình */
  display: flex;           /* Dùng flexbox */
  flex-direction: column;  /* Sắp xếp dọc từ trên xuống */
  box-sizing: border-box;  /* Để padding không làm vỡ layout */
}

/* Header và Tabs giữ nguyên */
.header { text-align: center; margin-bottom: 30px; }
h1 { margin-bottom: 5px; color: #2c3e50; }
.subtitle { color: #666; margin-top: 0; }

/* 2. Thêm class .content và cho nó giãn nở tối đa */
.content {
  flex: 1; /* Quan trọng: Nó sẽ chiếm hết khoảng trống thừa để đẩy footer xuống */
}

/* 3. Footer giữ nguyên (hoặc chỉnh margin-top nếu muốn) */
.app-footer {
  margin-top: 50px; /* Cách nội dung một chút cho thoáng */
  padding-top: 20px;
  border-top: 1px solid #eee;
  text-align: center;
  color: #64748b;
}

.app-footer strong { color: #42b883; }
.small-text { font-size: 0.8em; opacity: 0.7; }
</style>