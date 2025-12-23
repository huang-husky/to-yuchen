<script setup>
import { ref, onMounted } from 'vue'
import { Lock, KeyRound, Sparkles } from 'lucide-vue-next'

const emit = defineEmits(['success'])

const password = ref('')
const error = ref(false)
const shaking = ref(false)
const stars = ref([])

const correctPassword = 'yu_chen'

// 生成星星
const generateStars = () => {
  const starCount = 100
  const newStars = []
  for (let i = 0; i < starCount; i++) {
    newStars.push({
      x: Math.random() * 100,
      y: Math.random() * 100,
      size: Math.random() * 3 + 1,
      duration: Math.random() * 3 + 2,
      delay: Math.random() * 2
    })
  }
  stars.value = newStars
}

const handleSubmit = () => {
  if (password.value === correctPassword) {
    error.value = false
    emit('success')
  } else {
    error.value = true
    shaking.value = true
    setTimeout(() => {
      shaking.value = false
    }, 500)
    password.value = ''
  }
}

onMounted(() => {
  generateStars()
})
</script>

<template>
  <div class="password-page">
    <!-- 星空背景 -->
    <div class="stars-container">
      <div 
        v-for="(star, index) in stars" 
        :key="index"
        class="star"
        :style="{
          left: star.x + '%',
          top: star.y + '%',
          width: star.size + 'px',
          height: star.size + 'px',
          animationDuration: star.duration + 's',
          animationDelay: star.delay + 's'
        }"
      ></div>
    </div>

    <!-- 流星 -->
    <div class="shooting-star" style="top: 20%; left: 10%; animation-delay: 2s;"></div>
    <div class="shooting-star" style="top: 40%; left: 80%; animation-delay: 5s;"></div>
    <div class="shooting-star" style="top: 70%; left: 30%; animation-delay: 8s;"></div>

    <!-- 飘落绿叶 -->
    <div v-for="n in 15" :key="'leaf-' + n" class="falling-leaf" 
         :style="{
           left: Math.random() * 100 + '%',
           animationDuration: (Math.random() * 5 + 8) + 's',
           animationDelay: Math.random() * 5 + 's'
         }">
    </div>

    <div class="content" :class="{ shake: shaking }">
      <!-- 顶部装饰 -->
      <div class="top-decoration">
        <Sparkles :size="24" class="sparkle left" />
        <div class="decoration-line"></div>
        <Sparkles :size="24" class="sparkle right" />
      </div>

      <!-- 锁图标带光晕 -->
      <div class="lock-container">
        <div class="lock-glow"></div>
        <Lock :size="70" :stroke-width="1.5" class="lock-icon" />
      </div>

      <h1 class="title">To Chen Yu</h1>
      <p class="poem">「 So much to say, yet words fall short. 」</p>
      <p class="subtitle">please enter password</p>

      <!-- 密码输入框 -->
      <div class="input-wrapper">
        <div class="input-container">
          <KeyRound :size="20" class="input-icon" />
          <input
            v-model="password"
            type="password"
            placeholder="请输入密码"
            @keyup.enter="handleSubmit"
            class="password-input"
            :class="{ error: error }"
          />
        </div>
        <div class="input-border"></div>
      </div>

      <p v-if="error" class="error-text">❌ Wrong pwd! Please try again.</p>

      <button @click="handleSubmit" class="submit-btn">
        <span>Unlock</span>
        <div class="btn-shine"></div>
      </button>

      <div class="hint-box">
        <span class="hint-icon">💡</span>
        <span class="hint-text">by hxq</span>
      </div>

      <!-- 底部装饰 -->
      <div class="bottom-decoration">
        <div class="decoration-dot"></div>
        <div class="decoration-dot"></div>
        <div class="decoration-dot"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@font-face {
  font-family: 'WenDao';
  src: url('/fonts/WenDao.ttf') format('truetype');
}

.password-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #0a1f0f 0%, #0f2818 50%, #1a3a28 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
  font-family: 'WenDao', 'KaiTi', '楷体', serif;
}

/* 星空效果 */
.stars-container {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.star {
  position: absolute;
  background: #b8e6d5;
  border-radius: 50%;
  box-shadow: 0 0 3px rgba(184, 230, 213, 0.8);
  animation: twinkle infinite ease-in-out;
}

@keyframes twinkle {
  0%, 100% { 
    opacity: 0.4; 
    transform: scale(1);
  }
  50% { 
    opacity: 1; 
    transform: scale(1.3);
    box-shadow: 0 0 8px rgba(184, 230, 213, 0.9);
  }
}

/* 流星效果 */
.shooting-star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: #a8d8c3;
  border-radius: 50%;
  box-shadow: 0 0 10px 2px rgba(168, 216, 195, 0.8);
  animation: shootingStar 3s linear infinite;
}

@keyframes shootingStar {
  0% {
    transform: translate(0, 0) scale(1);
    opacity: 1;
  }
  100% {
    transform: translate(-300px, 300px) scale(0);
    opacity: 0;
  }
}

/* 飘落绿叶 */
.falling-leaf {
  position: fixed;
  width: 18px;
  height: 18px;
  background: linear-gradient(135deg, #4a9d5f 0%, #5fb878 50%, #7ecf8d 100%);
  border-radius: 0 50% 50% 50%;
  animation: leafFall linear infinite;
  z-index: 1;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.falling-leaf::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #5fb878 0%, #7ecf8d 100%);
  border-radius: 50% 0 50% 50%;
  transform: rotate(90deg);
  opacity: 0.7;
}

@keyframes leafFall {
  0% {
    transform: translateY(-100px) rotate(0deg);
    opacity: 0.8;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) rotate(360deg);
    opacity: 0.5;
  }
}

/* 内容卡片 */
.content {
  position: relative;
  z-index: 10;
  background: rgba(255, 255, 255, 0.98);
  padding: 50px 45px;
  border-radius: 25px;
  box-shadow: 
    0 25px 80px rgba(0, 0, 0, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(15px);
  text-align: center;
  max-width: 480px;
  width: 90%;
  animation: fadeIn 1s ease-out;
  border: 1px solid rgba(74, 157, 95, 0.3);
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(40px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

/* 顶部装饰 */
.top-decoration {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin-bottom: 25px;
  opacity: 0.6;
}

.sparkle {
  color: #3a7d44;
  animation: sparkleRotate 4s linear infinite;
}

.sparkle.left {
  animation-delay: 0s;
}

.sparkle.right {
  animation-delay: 2s;
}

@keyframes sparkleRotate {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(90deg); }
  50% { transform: rotate(180deg); }
  75% { transform: rotate(270deg); }
}

.decoration-line {
  width: 100px;
  height: 2px;
  background: linear-gradient(90deg, transparent, #3a7d44, transparent);
}

/* 锁图标容器 */
.lock-container {
  position: relative;
  width: 120px;
  height: 120px;
  margin: 0 auto 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.lock-glow {
  position: absolute;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(58, 125, 68, 0.3), transparent 70%);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 0.5;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.8;
  }
}

.lock-icon {
  color: #2d5016;
  position: relative;
  z-index: 2;
  filter: drop-shadow(0 0 10px rgba(45, 80, 22, 0.5));
  animation: lockFloat 3s ease-in-out infinite;
}

@keyframes lockFloat {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-8px);
  }
}

.title {
  font-size: 36px;
  background: linear-gradient(135deg, #2d5016, #3a7d44, #4a9d5f);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 12px;
  font-weight: bold;
  letter-spacing: 4px;
}

.poem {
  font-size: 16px;
  color: #5a7d5a;
  margin-bottom: 8px;
  font-style: italic;
  letter-spacing: 2px;
}

.subtitle {
  font-size: 15px;
  color: #6b8e6b;
  margin-bottom: 35px;
  letter-spacing: 1px;
}

/* 输入框包装器 */
.input-wrapper {
  position: relative;
  margin-bottom: 12px;
}

.input-container {
  position: relative;
  z-index: 2;
}

.input-icon {
  position: absolute;
  left: 18px;
  top: 50%;
  transform: translateY(-50%);
  color: #7d9d7d;
  z-index: 3;
}

.password-input {
  width: 100%;
  padding: 16px 18px 16px 50px;
  border: 2px solid #d4e8d4;
  border-radius: 12px;
  font-size: 16px;
  transition: all 0.3s;
  box-sizing: border-box;
  background: rgba(255, 255, 255, 0.9);
  font-family: 'WenDao', 'KaiTi', '楷体', serif;
}

.password-input:focus {
  outline: none;
  border-color: #3a7d44;
  background: white;
  box-shadow: 0 0 0 4px rgba(58, 125, 68, 0.1);
}

.password-input.error {
  border-color: #c74a4a;
  animation: shake 0.5s;
}

.input-border {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #2d5016, #3a7d44);
  transition: width 0.3s;
  z-index: 1;
}

.password-input:focus ~ .input-border {
  width: 100%;
}

.error-text {
  color: #c74a4a;
  font-size: 14px;
  margin-bottom: 20px;
  font-weight: 500;
}

/* 提交按钮 */
.submit-btn {
  position: relative;
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, #2d5016 0%, #3a7d44 50%, #2d5016 100%);
  background-size: 200% 100%;
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.4s;
  margin-bottom: 20px;
  overflow: hidden;
  letter-spacing: 6px;
  font-family: 'WenDao', 'KaiTi', '楷体', serif;
  box-shadow: 0 8px 20px rgba(45, 80, 22, 0.3);
}

.submit-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 30px rgba(45, 80, 22, 0.4);
  background-position: 100% 0;
}

.submit-btn:active {
  transform: translateY(-1px);
}

.btn-shine {
  position: absolute;
  top: -50%;
  left: -100%;
  width: 30%;
  height: 200%;
  background: rgba(255, 255, 255, 0.3);
  transform: rotate(25deg);
  animation: btnShine 3s infinite;
}

@keyframes btnShine {
  0% {
    left: -100%;
  }
  20%, 100% {
    left: 150%;
  }
}

/* 提示框 */
.hint-box {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: linear-gradient(135deg, #f0f8f0, #e6f2e6);
  padding: 10px 20px;
  border-radius: 20px;
  border: 1px solid #c8e0c8;
  margin-bottom: 25px;
}

.hint-icon {
  font-size: 18px;
}

.hint-text {
  font-size: 14px;
  color: #3a5d3a;
  letter-spacing: 1px;
}

/* 底部装饰 */
.bottom-decoration {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 15px;
}

.decoration-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: linear-gradient(135deg, #2d5016, #3a7d44);
  animation: dotBounce 1.5s ease-in-out infinite;
}

.decoration-dot:nth-child(2) {
  animation-delay: 0.2s;
}

.decoration-dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes dotBounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-8px);
  }
}

/* 抖动动画 */
.shake {
  animation: shake 0.5s;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-12px); }
  75% { transform: translateX(12px); }
}

/* 响应式 */
@media (max-width: 768px) {
  .content {
    padding: 40px 30px;
    margin: 20px;
  }
  
  .title {
    font-size: 28px;
  }
}
</style>