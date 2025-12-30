<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

// --- PHẦN 1: ĐỒNG HỒ REAL-TIME ---
const currentUnix = ref(Math.floor(Date.now() / 1000))
let timer = null

onMounted(() => {
  timer = setInterval(() => {
    currentUnix.value = Math.floor(Date.now() / 1000)
  }, 1000)
})

onUnmounted(() => {
  if (timer) clearInterval(timer)
})

const copyToClipboard = (text) => {
  navigator.clipboard.writeText(text)
  // Có thể thêm toast thông báo ở đây nếu muốn
}

// --- PHẦN 2: CONVERTER (Timestamp -> Date) ---
const inputTs = ref('')
const outputDate = ref(null)

const convertToDate = () => {
  if (!inputTs.value) return
  const dateObj = new Date(Number(inputTs.value) * 1000)
  
  if (isNaN(dateObj.getTime())) {
    outputDate.value = "Timestamp không hợp lệ!"
  } else {
    outputDate.value = dateObj.toLocaleString('vi-VN', { 
      timeZoneName: 'short', hour12: false 
    }) + ` (ISO: ${dateObj.toISOString()})`
  }
}

// --- PHẦN 3: DATE RANGE (Start/End of Day) - MỚI THÊM ---
// Hàm lấy ngày hiện tại format YYYY-MM-DD theo giờ địa phương
const getTodayString = () => {
  const d = new Date()
  const year = d.getFullYear()
  const month = String(d.getMonth() + 1).padStart(2, '0')
  const day = String(d.getDate()).padStart(2, '0')
  return `${year}-${month}-${day}`
}

const selectedDate = ref(getTodayString())

// Tự động tính toán khi selectedDate thay đổi
const rangeTimestamps = computed(() => {
  if (!selectedDate.value) return { start: '...', end: '...' }

  const dateStr = selectedDate.value
  // Tạo mốc 00:00:00
  const startDate = new Date(`${dateStr}T00:00:00`)
  // Tạo mốc 23:59:59
  const endDate = new Date(`${dateStr}T23:59:59`)

  return {
    start: Math.floor(startDate.getTime() / 1000),
    end: Math.floor(endDate.getTime() / 1000)
  }
})
</script>

<template>
  <div class="tool-container">
    <h3>Unix Timestamp Converter</h3>

    <div class="live-clock">
      <div class="clock-label">Hiện tại (Unix Timestamp):</div>
      <div class="clock-value">{{ currentUnix }}</div>
      <div class="human-time">
        {{ new Date(currentUnix * 1000).toLocaleString('vi-VN') }}
      </div>
      <button class="copy-btn" @click="copyToClipboard(currentUnix)">Copy Live</button>
    </div>

    <div class="range-box">
      <h4>📅 Lấy Timestamp Đầu ngày / Cuối ngày</h4>
      <div class="date-input-group">
        <label>Chọn ngày:</label>
        <input type="date" v-model="selectedDate" class="ts-input" />
      </div>

      <div class="range-results">
        <div class="range-item">
          <span class="label">Start Day (00:00:00):</span>
          <div class="value-row">
            <code>{{ rangeTimestamps.start }}</code>
            <button class="copy-btn-small" @click="copyToClipboard(rangeTimestamps.start)">Copy</button>
          </div>
        </div>

        <div class="range-item">
          <span class="label">End Day (23:59:59):</span>
          <div class="value-row">
            <code>{{ rangeTimestamps.end }}</code>
            <button class="copy-btn-small" @click="copyToClipboard(rangeTimestamps.end)">Copy</button>
          </div>
        </div>
      </div>
    </div>

    <hr />

    <div class="converter-box">
      <h4>🔄 Chuyển đổi (Timestamp sang Ngày)</h4>
      <div style="display: flex; gap: 10px; margin-bottom: 10px;">
        <input type="number" v-model="inputTs" placeholder="Nhập timestamp..." class="ts-input" />
        <button class="action-btn" @click="convertToDate">Xem ngày</button>
      </div>
      <div v-if="outputDate" class="result-box">
        <strong>Kết quả:</strong> {{ outputDate }}
      </div>
    </div>

  </div>
</template>

<style scoped>
/* Style cũ giữ nguyên */
.live-clock {
  background: #2c3e50; color: white; padding: 20px; border-radius: 8px; text-align: center; margin-bottom: 20px;
}
.clock-value { font-size: 2.5em; font-weight: bold; font-family: monospace; color: #42b883; }
.human-time { margin-top: 5px; font-size: 0.9em; color: #ccc; }
.copy-btn { margin-top: 10px; padding: 5px 15px; cursor: pointer; background: #42b883; border: none; border-radius: 4px; color: white; font-weight: bold; }
.copy-btn:active { transform: translateY(1px); }

.ts-input { padding: 8px; border: 1px solid #ccc; border-radius: 4px; font-size: 14px; }
.action-btn { padding: 8px 16px; background: #2c3e50; color: white; border: none; border-radius: 4px; cursor: pointer; }

/* Style mới cho phần Range */
.range-box {
  background: #f0fdf4; /* Màu xanh nhạt */
  border: 1px solid #bbf7d0;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}
.date-input-group { display: flex; align-items: center; gap: 10px; margin-bottom: 15px; }
.range-results { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
.range-item { background: white; padding: 10px; border-radius: 6px; border: 1px solid #ddd; }
.range-item .label { font-size: 0.85em; color: #666; display: block; margin-bottom: 5px; }
.value-row { display: flex; justify-content: space-between; align-items: center; }
.value-row code { font-weight: bold; font-size: 1.1em; color: #d63384; }
.copy-btn-small { padding: 2px 8px; font-size: 0.8em; cursor: pointer; border: 1px solid #ccc; background: #fff; border-radius: 3px; }
.copy-btn-small:hover { background: #eee; }

@media (max-width: 600px) {
  .range-results { grid-template-columns: 1fr; }
}

hr { border: 0; border-top: 1px solid #eee; margin: 20px 0; }
</style>