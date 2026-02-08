<script setup>
import { ref, computed } from 'vue'

const recipient = ref('')
const currentGreeting = ref('點擊下方按鈕生成吉祥話！')
const selectedCategory = ref('general')

const greetings = {
  general: [
    '祝您新年快樂，萬事如意！',
    '恭喜發財，紅包拿來！',
    '歲歲平安，年年有餘！',
    '新春大吉，闔家安康！',
    '五福臨門，吉星高照！'
  ],
  wealth: [
    '大吉大利，財源廣進！',
    '招財進寶，富貴滿堂！',
    '生意興隆通四海，財源茂盛達三江！',
    '金玉滿堂，財運亨通！',
    '財神敲門，好運連連！'
  ],
  health: [
    '龍馬精神，身體健康！',
    '福壽雙全，平安喜樂！',
    '笑口常開，青春永駐！',
    '生龍活虎，體魄強健！',
    '長命百歲，歲歲安康！'
  ],
  career: [
    '步步高升，平步青雲！',
    '前程似錦，大展鴻圖！',
    '事業有成，一帆風順！',
    '升官發財，名利雙收！',
    '工作順利，職位躍升！'
  ]
}

const generateGreeting = () => {
  const categoryGreetings = greetings[selectedCategory.value]
  const randomIndex = Math.floor(Math.random() * categoryGreetings.length)
  let text = categoryGreetings[randomIndex]
  
  if (recipient.value.trim()) {
    text = `${recipient.value}，${text}`
  }
  
  currentGreeting.value = text
}

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(currentGreeting.value)
    alert('已複製到剪貼簿！')
  } catch (err) {
    alert('複製失敗，請手動選取文字複製。')
  }
}

const categories = [
  { id: 'general', name: '通用祝福' },
  { id: 'wealth', name: '財源廣進' },
  { id: 'health', name: '身體健康' },
  { id: 'career', name: '事業有成' }
]
</script>

<template>
  <div class="cny-card">
    <h1 class="cny-title">🧧 新春吉祥話</h1>
    
    <div class="input-group">
      <label for="recipient">收信人姓名 (選填)</label>
      <input 
        id="recipient"
        v-model="recipient" 
        type="text" 
        class="cny-input" 
        placeholder="例如：王小明"
      />
    </div>

    <div class="category-selector">
      <label>選擇祝福類別</label>
      <div class="category-chips">
        <button 
          v-for="cat in categories" 
          :key="cat.id"
          :class="['chip', { active: selectedCategory === cat.id }]"
          @click="selectedCategory = cat.id"
        >
          {{ cat.name }}
        </button>
      </div>
    </div>

    <button class="cny-button" @click="generateGreeting">
      ✨ 產生吉祥話 ✨
    </button>

    <div class="greeting-display" @click="copyToClipboard" title="點擊複製">
      {{ currentGreeting }}
    </div>
    
    <p class="hint">💡 點擊上方文字即可複製到剪貼簿</p>

    <!-- Decorative Elements -->
    <div class="lantern lantern-left">🏮</div>
    <div class="lantern lantern-right">🏮</div>
  </div>
</template>

<style scoped>
.input-group {
  text-align: left;
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: #666;
  font-size: 0.9rem;
}

.category-selector {
  margin-bottom: 1.5rem;
  text-align: left;
}

.category-chips {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-top: 0.5rem;
}

.chip {
  padding: 0.4rem 1rem;
  border-radius: 20px;
  border: 1px solid #ddd;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 0.9rem;
}

.chip.active {
  background: var(--cny-red);
  color: white;
  border-color: var(--cny-red);
}

.hint {
  margin-top: 1rem;
  font-size: 0.8rem;
  color: #888;
}

.greeting-display {
  cursor: pointer;
  transition: background 0.3s;
}

.greeting-display:hover {
  background: rgba(211, 47, 47, 0.1);
}

/* Animations */
.lantern {
  position: absolute;
  font-size: 2rem;
  top: 1rem;
  animation: swing 3s ease-in-out infinite alternate;
}

.lantern-left {
  left: 1rem;
}

.lantern-right {
  right: 1rem;
}

@keyframes swing {
  from { transform: rotate(-5deg); }
  to { transform: rotate(5deg); }
}
</style>

