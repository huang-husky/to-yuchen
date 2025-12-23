<script setup>
import { ref } from 'vue'

const emit = defineEmits(['open'])

const isOpening = ref(false)

const handleClick = () => {
  if (!isOpening.value) {
    isOpening.value = true
    setTimeout(() => {
      emit('open')
    }, 2000)
  }
}
</script>

<template>
  <div class="scroll-page">
    <!-- 星空背景 -->
    <div class="stars-container">
      <div 
        v-for="n in 80" 
        :key="n"
        class="star"
        :style="{
          left: Math.random() * 100 + '%',
          top: Math.random() * 100 + '%',
          width: (Math.random() * 3 + 1) + 'px',
          height: (Math.random() * 3 + 1) + 'px',
          animationDuration: (Math.random() * 3 + 2) + 's'
        }"
      ></div>
    </div>

    <!-- 飘落绿叶 -->
    <div v-for="n in 12" :key="'leaf-' + n" class="falling-leaf" 
         :style="{
           left: Math.random() * 100 + '%',
           animationDuration: (Math.random() * 5 + 8) + 's',
           animationDelay: Math.random() * 5 + 's'
         }">
    </div>

    <div class="content">
      <h1 class="title">A Letter</h1>
      <p class="hint-top">轻触卷轴，开启阅读</p>

      <!-- 卷轴 -->
      <div 
        class="scroll-container" 
        :class="{ opening: isOpening }"
        @click="handleClick"
      >
        <!-- 左侧轴 -->
        <div class="scroll-rod left-rod">
          <div class="rod-cap top"></div>
          <div class="rod-cap bottom"></div>
        </div>
        
        <!-- 右侧轴 -->
        <div class="scroll-rod right-rod">
          <div class="rod-cap top"></div>
          <div class="rod-cap bottom"></div>
        </div>
        
        <!-- 卷轴纸（竹简风格） -->
        <div class="scroll-paper">
          <!-- 顶部装饰边 -->
          <div class="scroll-decoration top">
            <div class="decoration-pattern"></div>
          </div>
          
          <!-- 卷轴内容 -->
          <div class="scroll-content">
            <!-- 竖排文字 -->
            <div class="vertical-text-container">
              <div class="vertical-text main-text">
                <span>致</span>
                <span>敬</span>
                <span>爱</span>
                <span>的</span>
                <span>陈</span>
                <span>老</span>
                <span>师</span>
              </div>
              
              <!-- 装饰性竖线 -->
              <div class="decoration-lines">
                <div class="deco-line"></div>
                <div class="deco-line"></div>
              </div>
            </div>
            
            <!-- 水墨装饰 -->
            <div class="ink-decoration top-left"></div>
            <div class="ink-decoration bottom-right"></div>
          </div>
          
          <!-- 底部装饰边 -->
          <div class="scroll-decoration bottom">
            <div class="decoration-pattern"></div>
          </div>
        </div>

        <!-- 系带（绿色） -->
        <div class="scroll-ribbon">
          <div class="ribbon-knot"></div>
        </div>
        
        <!-- 封蜡印章效果（墨绿色） -->
        <div class="wax-seal">
          <div class="seal-pattern">缘</div>
        </div>
      </div>

      <p class="hint-bottom">✨ 点击卷轴展开阅读</p>
    </div>
  </div>
</template>

<style scoped>
@font-face {
  font-family: 'WenDao';
  src: url('/fonts/WenDao.ttf') format('truetype');
}

.scroll-page {
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
  animation: twinkle infinite ease-in-out;
}

@keyframes twinkle {
  0%, 100% { opacity: 0.4; }
  50% { opacity: 1; }
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
  100% {
    transform: translateY(100vh) rotate(360deg);
    opacity: 0.5;
  }
}

.content {
  position: relative;
  z-index: 10;
  text-align: center;
  animation: fadeIn 0.8s ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.title {
  font-size: 38px;
  color: #a8d8c3;
  margin-bottom: 8px;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  letter-spacing: 6px;
}

.subtitle {
  font-size: 16px;
  color: rgba(184, 230, 213, 0.9);
  margin-bottom: 10px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  letter-spacing: 2px;
}

.hint-top {
  font-size: 15px;
  color: rgba(184, 230, 213, 0.8);
  margin-bottom: 40px;
  letter-spacing: 2px;
}

/* 卷轴容器 */
.scroll-container {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: transform 0.3s;
  animation: float 3s ease-in-out infinite;
  filter: drop-shadow(0 15px 30px rgba(0, 0, 0, 0.5));
}

.scroll-container:hover {
  transform: scale(1.05);
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-15px);
  }
}

/* 卷轴轴（竹筒颜色） */
.scroll-rod {
  position: absolute;
  width: 22px;
  height: 400px;
  background: linear-gradient(90deg, #3d5a3d 0%, #4a6d4a 30%, #415e41 50%, #3d5a3d 100%);
  border-radius: 11px;
  box-shadow: 
    inset -3px 0 8px rgba(0, 0, 0, 0.4),
    inset 3px 0 8px rgba(74, 109, 74, 0.3);
  z-index: 20;
}

.left-rod {
  left: -11px;
  top: 0;
}

.right-rod {
  right: -11px;
  top: 0;
}

.rod-cap {
  position: absolute;
  width: 28px;
  height: 18px;
  left: 50%;
  transform: translateX(-50%);
  background: radial-gradient(ellipse, #2d4a2d 0%, #1f3a1f 100%);
  z-index: 1;
}

.rod-cap.top {
  top: -9px;
  border-radius: 50% 50% 0 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.rod-cap.bottom {
  bottom: -9px;
  border-radius: 0 0 50% 50%;
  box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.3);
}

/* 卷轴纸（略带竹青色） */
.scroll-paper {
  width: 300px;
  height: 400px;
  background: 
    linear-gradient(180deg, 
      #f5f8f0 0%, 
      #e8f0e0 20%,
      #dae8cd 50%, 
      #e8f0e0 80%,
      #f5f8f0 100%
    );
  border-radius: 5px;
  box-shadow: 
    0 20px 50px rgba(0, 0, 0, 0.4),
    inset 0 0 100px rgba(168, 216, 195, 0.1);
  position: relative;
  z-index: 10;
  overflow: hidden;
  border-left: 3px solid #a8d8c3;
  border-right: 3px solid #a8d8c3;
}

/* 装饰边 */
.scroll-decoration {
  width: 100%;
  height: 35px;
  background: repeating-linear-gradient(
    90deg,
    #8bb896 0px,
    #a8d8c3 8px,
    #c8e8d8 16px,
    #a8d8c3 24px,
    #8bb896 32px
  );
  position: relative;
  border-bottom: 2px solid #6d9d7d;
}

.scroll-decoration.top {
  border-bottom: 2px solid #6d9d7d;
}

.scroll-decoration.bottom {
  position: absolute;
  bottom: 0;
  border-top: 2px solid #6d9d7d;
  border-bottom: none;
}

.decoration-pattern {
  position: absolute;
  width: 100%;
  height: 100%;
  background: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 10px,
    rgba(109, 157, 125, 0.1) 10px,
    rgba(109, 157, 125, 0.1) 20px
  );
}

/* 卷轴内容 */
.scroll-content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: calc(100% - 70px);
  padding: 30px;
  position: relative;
}

/* 竖排文字容器 */
.vertical-text-container {
  display: flex;
  gap: 40px;
  align-items: center;
}

.vertical-text {
  writing-mode: vertical-rl;
  text-orientation: upright;
  letter-spacing: 0.15em;
  line-height: 1.8;
}

.main-text {
  font-size: 34px;
  font-weight: bold;
  color: #2d5016;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
  letter-spacing: 0.08em;
}

.main-text span {
  display: inline-block;
  animation: textReveal 0.5s ease-out backwards;
}

.main-text span:nth-child(1) { animation-delay: 0.1s; }
.main-text span:nth-child(2) { animation-delay: 0.2s; }
.main-text span:nth-child(3) { animation-delay: 0.3s; }
.main-text span:nth-child(4) { animation-delay: 0.4s; }
.main-text span:nth-child(5) { animation-delay: 0.5s; }
.main-text span:nth-child(6) { animation-delay: 0.6s; }
.main-text span:nth-child(7) { animation-delay: 0.7s; }

@keyframes textReveal {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 装饰线条 */
.decoration-lines {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.deco-line {
  width: 3px;
  height: 120px;
  background: linear-gradient(180deg, transparent, #8bb896, transparent);
  border-radius: 2px;
}

/* 水墨装饰 */
.ink-decoration {
  position: absolute;
  width: 80px;
  height: 80px;
  opacity: 0.08;
  background: radial-gradient(circle, #2d5016, transparent);
  border-radius: 50%;
}

.ink-decoration.top-left {
  top: 10px;
  left: 10px;
}

.ink-decoration.bottom-right {
  bottom: 10px;
  right: 10px;
}

/* 系带（深绿色） */
.scroll-ribbon {
  position: absolute;
  width: 320px;
  height: 12px;
  background: linear-gradient(90deg, transparent, #2d5016, #3a7d44, #2d5016, transparent);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 25;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.4);
}

.ribbon-knot {
  position: absolute;
  width: 35px;
  height: 35px;
  background: radial-gradient(circle, #3a7d44, #2d5016);
  border-radius: 50%;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 
    0 3px 10px rgba(0, 0, 0, 0.5),
    inset -2px -2px 5px rgba(0, 0, 0, 0.3);
}

.ribbon-knot::before,
.ribbon-knot::after {
  content: '';
  position: absolute;
  width: 20px;
  height: 25px;
  background: linear-gradient(135deg, #3a7d44, #2d5016);
  border-radius: 50% 50% 0 50%;
  top: 50%;
}

.ribbon-knot::before {
  left: -15px;
  transform: translateY(-50%) rotate(-45deg);
}

.ribbon-knot::after {
  right: -15px;
  transform: translateY(-50%) rotate(45deg);
}

/* 封蜡印章（墨绿色） */
.wax-seal {
  position: absolute;
  width: 50px;
  height: 50px;
  background: radial-gradient(circle, #3a7d44, #2d5016);
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 30;
  box-shadow: 
    0 4px 12px rgba(0, 0, 0, 0.5),
    inset 0 0 15px rgba(45, 80, 22, 0.5);
  border: 2px solid rgba(45, 80, 22, 0.8);
}

.seal-pattern {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  color: rgba(255, 255, 255, 0.9);
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
}

/* 展开动画 */
.opening .scroll-paper {
  animation: unrollScroll 2s ease-out forwards;
}

.opening .scroll-ribbon,
.opening .wax-seal {
  animation: fadeOut 0.6s ease-out forwards;
}

.opening .left-rod {
  animation: rollLeft 2s ease-out forwards;
}

.opening .right-rod {
  animation: rollRight 2s ease-out forwards;
}

@keyframes unrollScroll {
  0% {
    transform: scaleX(1);
  }
  100% {
    transform: scaleX(3) scaleY(1.3);
    opacity: 0;
  }
}

@keyframes fadeOut {
  to {
    opacity: 0;
    transform: scale(0);
  }
}

@keyframes rollLeft {
  0% {
    transform: translateX(0) rotate(0deg);
  }
  100% {
    transform: translateX(-180px) rotate(-1080deg);
    opacity: 0;
  }
}

@keyframes rollRight {
  0% {
    transform: translateX(0) rotate(0deg);
  }
  100% {
    transform: translateX(180px) rotate(1080deg);
    opacity: 0;
  }
}

.hint-bottom {
  font-size: 16px;
  color: #a8d8c3;
  margin-top: 45px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  animation: pulse 2s ease-in-out infinite;
  letter-spacing: 2px;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
}

/* 响应式 */
@media (max-width: 768px) {
  .scroll-paper {
    width: 260px;
    height: 360px;
  }
  
  .scroll-rod {
    height: 360px;
  }
  
  .main-text {
    font-size: 36px;
  }
  
  .title {
    font-size: 32px;
  }
}
</style>