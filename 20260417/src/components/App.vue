<template>
  <div :class="['app-container', { 'dark-mode': isDarkMode, 'phonetic-mode': isPhonetic }]">
    <!-- 頂部標題區 -->
    <header class="header">
      <div class="header-left">
        <a href="https://www.et.tku.edu.tw/" target="_blank" class="nav-btn">淡江教科系</a>
      </div>
      <h1 class="title">
        <ruby v-if="isPhonetic">互動程式設計<rt>ㄏㄨˋ ㄉㄨㄥˋ ㄔㄥˊ ㄕˋ ㄕㄜˋ ㄐㄧˋ</rt></ruby>
        <span v-else>互動程式設計</span>_414737139
      </h1>
      <div class="header-right"></div> <!-- 保持平衡 -->
    </header>

    <!-- 監考工具按鈕區 -->
    <section class="tools-section">
      <button @click="toggleTimeMode" class="tool-btn">
        {{ isCountdown ? '切換現在時間' : '切換倒數計時' }}
      </button>
      <button @click="isPhonetic = !isPhonetic" class="tool-btn">
        {{ isPhonetic ? '隱藏注音' : '顯示注音' }}
      </button>
      <button @click="isDarkMode = !isDarkMode" class="tool-btn">
        {{ isDarkMode ? '切換白底黑字' : '切換黑底白字' }}
      </button>
    </section>

    <!-- 時間顯示區 -->
    <main class="display-section">
      <div class="time-display">
        <h2 class="display-label">{{ isCountdown ? '剩餘時間' : '現在時間' }}</h2>
        <div class="time-digits">{{ displayTime }}</div>
      </div>

      <!-- 考試資訊輸入區 -->
      <div class="info-form card">
        <h3>考試資訊設定</h3>
        <div class="input-group">
          <label>考試科目：</label>
          <input v-model="examData.subject" type="text" placeholder="請輸入科目">
        </div>
        <div class="input-group">
          <label>監考老師：</label>
          <input v-model="examData.proctor" type="text" placeholder="請輸入姓名">
        </div>
        <div class="input-group" v-if="isCountdown">
          <label>設定分鐘：</label>
          <input v-model.number="inputMinutes" type="number" min="1" max="180">
          <button @click="startCountdown" class="small-btn">開始計時</button>
        </div>
      </div>

      <!-- 考試資訊顯示區 -->
      <div class="info-display card" v-if="examData.subject || examData.proctor">
        <h3>當前監考資訊</h3>
        <p><strong>科目：</strong>{{ examData.subject || '未設定' }}</p>
        <p><strong>監考老師：</strong>{{ examData.proctor || '未設定' }}</p>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

// 狀態設定
const isDarkMode = ref(false);
const isPhonetic = ref(false);
const isCountdown = ref(false);
const currentTime = ref(new Date());
const countdownSeconds = ref(0);
const inputMinutes = ref(60);
const examData = ref({
  subject: '',
  proctor: ''
});

let timerInterval = null;

// 時間格式化邏輯
const formatTime = (date) => {
  return date.toLocaleTimeString('zh-TW', { hour12: false });
};

const formatCountdown = (totalSeconds) => {
  const hrs = Math.floor(totalSeconds / 3600);
  const mins = Math.floor((totalSeconds % 3600) / 60);
  const secs = totalSeconds % 60;
  return `${hrs.toString().padStart(2, '0')}:${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
};

const displayTime = computed(() => {
  return isCountdown.value ? formatCountdown(countdownSeconds.value) : formatTime(currentTime.value);
});

// 切換模式
const toggleTimeMode = () => {
  isCountdown.value = !isCountdown.value;
};

const startCountdown = () => {
  countdownSeconds.value = inputMinutes.value * 60;
};

// 計時器邏輯
onMounted(() => {
  timerInterval = setInterval(() => {
    currentTime.value = new Date();
    if (isCountdown.value && countdownSeconds.value > 0) {
      countdownSeconds.value--;
    }
  }, 1000);
});

onUnmounted(() => {
  clearInterval(timerInterval);
});
</script>

<style scoped>
/* 基礎佈局 */
.app-container {
  min-height: 100vh;
  padding: 20px;
  font-family: 'PingFang TC', 'Microsoft JhengHei', sans-serif;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #ffffff;
  color: #333333;
}

/* 黑底白字模式 */
.dark-mode {
  background-color: #1a1a1a;
  color: #ffffff;
}

/* 標題區 */
.header {
  width: 100%;
  max-width: 800px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  border-bottom: 2px solid #ddd;
  padding-bottom: 15px;
}

.title {
  font-size: 1.5rem;
  margin: 0;
  text-align: center;
}

.nav-btn {
  padding: 8px 15px;
  background-color: #004da0;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  font-size: 0.9rem;
}

/* 工具按鈕 */
.tools-section {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  flex-wrap: wrap;
  justify-content: center;
}

.tool-btn {
  padding: 10px 20px;
  cursor: pointer;
  border: 1px solid #004da0;
  background: transparent;
  color: inherit;
  border-radius: 8px;
  font-weight: bold;
  transition: 0.2s;
}

.tool-btn:hover {
  background-color: #004da0;
  color: white;
}

/* 時間顯示 */
.display-section {
  width: 100%;
  max-width: 600px;
  text-align: center;
}

.time-digits {
  font-size: 4rem;
  font-family: 'Courier New', Courier, monospace;
  font-weight: bold;
  margin: 20px 0;
}

/* 卡片樣式 */
.card {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 12px;
  margin-top: 20px;
  text-align: left;
}

.dark-mode .card {
  border-color: #444;
}

.input-group {
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.input-group input {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
  flex: 1;
}

.small-btn {
  padding: 8px 12px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* 手機響應式優化 */
@media (max-width: 600px) {
  .time-digits {
    font-size: 2.5rem;
  }
  .header {
    flex-direction: column;
    gap: 10px;
  }
}
</style>