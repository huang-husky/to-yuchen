<script setup>
import { ref, onMounted } from 'vue'
import { Volume2, VolumeX, Heart } from 'lucide-vue-next'

const musicPlaying = ref(false)
const audioElement = ref(null)
const showEasterEgg = ref(false)
const easterEggClicks = ref(0)

// 文字展示效果：'fadeIn' | 'slideIn' | 'scaleUp' | 'flipIn' | 'none'
const textAnimation = ref('fadeIn')

const visibleParagraphs = ref([])
const allParagraphsVisible = ref(false)

// 👇 the letter starts~
const paragraphsData = [
  '提笔写下这封信时，一学期的离散数学课程已近尾声。从九月的初秋到十二月的寒冬，每周一、四的两节课,如同一场思维的盛宴，在您春风化雨般的引导中，悄然重塑了我对计算机科学的认知。回望这段旅程，心中满是感激与不舍。',
  
  '犹记得第一节课，您并未急于翻开课本，而是先为我们“磨刀”。您说：“通过离散数学的学习，掌握证明问题的方法——特别是按定义证明，培养抽象思维的能力、缜密概括的能力和严密逻辑推理的能力。”那一刻，我意识到，这门计算机专业的基础课程，意义远不止于知识的传递，更是一场思维方式的塑造。磨刀不误砍柴工，这份用心的铺垫，为整个学期的学习定下了基调。',
  
  '您的课堂，总是充满温度与智慧。讲到偏序集的上界、下界时，抽象的概念在您口中变得生动起来。您用我们上课的教室作比喻：教室中的天花板及其以上都是我们的上界，而头顶的天花板便是最小上界。“但是，”您话锋一转，“假如班上有位同学站在桌子上，那么他虽然比教室里任何人都高，也同样是最小上界。”这样贴近生活的讲解，让晦涩的定义瞬间清晰。而在讲到图论中“基本道路”与“简单道路”的区别时，您拿逛景点和每天上下学两种路线作比：逛景点可以重复经过同一个地方但不走回头路，而上下学则往往是最熟悉的固定路线。这些看似随意的类比，却总能让我们在会心一笑中，抓住概念的本质。',
  
  '您教给我们的，不仅是知识，更是方法与智慧。讲到蕴涵律时，您说“没有条件，就创造条件”“万事俱备”；为了帮助我们理解闭包，您提到“缺啥补啥、过犹不及”；在谓词逻辑的推理证明中，您教我们变量要“省”着用，按位置，选变量；讲到割点时，您用“一夫当关，万夫莫开”形容那个被移除后会让图不连通的关键节点。这些凝练而精妙的表达，既帮助我们记住了抽象的定理，更让我们体会到，离散数学中处处蕴含着朴素而深刻的生活哲理。讲到哈夫曼树时，您不仅教我们如何构造最优二叉树，更借此传递做人的道理——选择权重的过程，就像人生的取舍，要懂得平衡与中庸，既不走极端，也不盲目求全。',
  
  '您的PPT制作得极为用心，这是我到目前见过最精心设计的课件。更妙的是，您会故意在其中留下一些“小错误”或不够严谨的表述，让我们在课堂上主动发现、思考、修正。这种留白的教学艺术，不仅提高了我们的听课效率，更培养了我们的批判性思维与自主学习能力。您还鼓励学有余力的同学去做PPT和书上的题目，这份信任与期待，让我们感受到被重视，也激发了更多的学习热情。',
  
  '作为计算机专业的基础课程，您深知离散数学不应止步于纸面的推理与证明。您常常鼓励我们在课后用代码实现相关概念：用程序生成真值表，让命题逻辑的运算变得直观可见；口述用程序实现哈斯图的思路，让偏序关系在代码中“活”起来；从编程的角度判断一个图是否为二部图，将理论与实践无缝衔接。这些引导，让我真切体会到，离散数学不是孤立的数学分支，而是计算机科学大厦的基石，是算法设计、数据结构、编译原理的共同语言。',
  
  '您还注重培养我们的计算机人文素养。讲到图论时，您带我们回到十八世纪的哥尼斯堡，从七桥问题中领略欧拉的智慧；讲到哈密尔顿回路时，您又带我们“周游世界”，在抽象的数学结构中，感受人类探索未知的浪漫。这些历史故事，让我们明白，计算机科学不仅是技术，更是人类思想演进的结晶。',
  
  '课堂之外，您更是我们坚实的后盾。这学期我给您发了近二十封邮件，从集合论的细节到代数系统的困惑，从图论的证明到群环域的理解，每一个疑问您都耐心解答，从不敷衍。我至今记得那封回复：您不仅肯定了我的笔记和思路，还细致地为我讲解最大元、最小元与最小上界、最大下界的区别。这份温暖与鼓励，让我在迷茫时看到了方向，也让我明白，真正的好老师，不仅传道授业，更懂得如何点燃学生心中那团求知的火焰。课间时分，我也常抽空向您请教，您总是放下手中的事，友善而耐心地为我解惑。这些点滴的关怀，都深深铭刻在我心中。',
  
  '您常说：要善于挖掘隐含条件，找生成元时往往从最小的元素开始……这些做题的小技巧，看似简单，却蕴含着深刻的逻辑洞察力。您的补充题也从不局限于应试，而是精心挑选那些能开拓思路、启发思维的题目，让我们在解题中感受到离散数学的魅力与深度。',
  
  '这一学期，我们从命题逻辑走到谓词逻辑，再到集合论、函数关系、图论，从代数系统到群、环、域，如今又即将学习格与布尔代数。每一步都是知识的扩展，也是思维的升华。学到幺元、逆元、封闭性等概念时，我忽然有了一种“升维”的感觉：原来中学学过的加减乘除、方程变换，都是更宏大数学结构中的特例；而离散数学，正是将这些零散的知识点串联起来，构建成一个完整而优美的体系。正如您所说，学习任何学科，都要构建整体的框架，从宏观层面把握各知识点之间的关系，进而细化深入。这份启迪，将使我受益终身。',
  
  '您学识深厚，却从不高高在上；您慈祥和蔼，却不失严谨认真。在您身上，我看到了优秀教师的典范：既有深厚的学术功底，又有温润的教育情怀；既注重知识的传递，更重视思维的培养；既立足理论，又引导实践。您让我明白，真正的教育，不是填鸭式的灌输，而是点燃思维的火花，引导学生在求知的路上越走越远。',
  
  '回望这一学期，您的诸多期许与要求，我虽未能完全达到，但这份差距，恰是我继续前行的方向。您播下的种子，终会在日后的学习中生根发芽。我相信，那些尚未做到的，都会在不断的精进中逐步实现。',

  '纸短情长，师恩难忘。一学期的离散数学课，您教会我的，远不止那些定理与证明，更是一种思考问题的方式，一种面对困难的态度，一种追求真理的执着。在这即将短暂分别的时刻，我想用我最喜欢的一首诗与您共勉：“花未全开月未圆，半山微醉尽余欢。何须多虑盈亏事，终归小满胜万全。”人生如花未全开月未圆，学习也是如此，无需苛求圆满，只需在每一步中收获成长与喜悦。您教会我的，正是这份从容与豁达。',
  
  '愿您在教书育人的道路上，继续播撒智慧的种子；愿您的生活如您钟爱的离散数学般严谨而优雅；愿您的每一次付出都被深深铭记，每一份关怀都温暖人心。感恩相遇，师恩难忘！',
]

const showParagraphs = () => {
  if (textAnimation.value === 'none') {
    visibleParagraphs.value = paragraphsData.map((_, index) => index)
    allParagraphsVisible.value = true
  } else {
    let index = 0
    const showNext = () => {
      if (index < paragraphsData.length) {
        visibleParagraphs.value.push(index)
        index++
        setTimeout(showNext, 600)
      } else {
        allParagraphsVisible.value = true
      }
    }
    setTimeout(showNext, 300)
  }
}

const toggleMusic = () => {
  if (!audioElement.value) return
  if (musicPlaying.value) {
    audioElement.value.pause()
  } else {
    audioElement.value.play()
  }
  musicPlaying.value = !musicPlaying.value
}

const handleCornerClick = () => {
  easterEggClicks.value++
  if (easterEggClicks.value >= 3) {
    showEasterEgg.value = true
    setTimeout(() => {
      showEasterEgg.value = false
      easterEggClicks.value = 0
    }, 3000)
  }
}

onMounted(() => {
  showParagraphs()
})
</script>

<template>
  <div class="letter-page">
    <!-- 星空背景 -->
    <div class="stars-container">
      <div 
        v-for="n in 120" 
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
    <div v-for="n in 25" :key="'leaf-' + n" class="falling-leaf" 
         :style="{
           left: Math.random() * 100 + '%',
           animationDuration: (Math.random() * 5 + 8) + 's',
           animationDelay: Math.random() * 5 + 's'
         }">
    </div>

    <!-- 音频元素（可选） -->
    <audio ref="audioElement" loop>
      <source src="/starRiver.mp3" type="audio/mpeg">
    </audio> 

    <!-- 音乐控制按钮（可选） -->
    <button class="music-control" @click="toggleMusic">
      <Volume2 v-if="musicPlaying" :size="24" />
      <VolumeX v-else :size="24" />
    </button> 

    <!-- 彩蛋触发区域 -->
    <div class="easter-egg-trigger" @click="handleCornerClick"></div>

    <!-- 彩蛋显示 -->
    <Transition name="fade">
      <div v-if="showEasterEgg" class="easter-egg">
        <Heart :size="100" class="heart-icon" />
        <p>谢谢陈老师，感恩相遇！🌿</p>
      </div>
    </Transition>

    <!-- 信纸卡片 -->
    <div class="letter-container">
      <!-- 顶部装饰 -->
      <div class="letter-header">
        <div class="header-decoration"></div>
        <div class="header-pattern"></div>
      </div>

      <!-- 信纸纹理背景 -->
      <div class="paper-texture"></div>

      <!-- 左侧装饰边框 -->
      <div class="side-decoration left">
        <div class="deco-line"></div>
        <div class="deco-flower"></div>
      </div>

      <!-- 右侧装饰边框 -->
      <div class="side-decoration right">
        <div class="deco-line"></div>
        <div class="deco-flower"></div>
      </div>

      <!-- 主内容区 -->
      <div class="letter-main">
        <h1 class="letter-title">
          <span class="title-char">致</span>
          <span class="title-char">敬</span>
          <span class="title-char">爱</span>
          <span class="title-char">的</span>
          <span class="title-char">陈</span>
          <span class="title-char">老</span>
          <span class="title-char">师</span>
        </h1>
        
        <div class="letter-content">
          <p 
            v-for="(para, index) in paragraphsData" 
            :key="index" 
            class="letter-paragraph"
            :class="[
              textAnimation,
              { 'is-visible': visibleParagraphs.includes(index) }
            ]"
            :style="{ animationDelay: (index * 0.2) + 's' }"
          >
            {{ para }}
          </p>
        </div>

        <div class="letter-signature" :class="{ 'is-visible': allParagraphsVisible }">
          <div class="signature-line">您的学生敬上</div>
          <div class="signature-name">黄湘淇</div>
          <div class="signature-date">2025年12月</div>
        </div>
      </div>

      <!-- 底部装饰 -->
      <div class="letter-footer">
        <div class="footer-pattern"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@font-face {
  font-family: 'WenDao';
  src: url('/fonts/WenDao.ttf') format('truetype');
}

.letter-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #0a1f0f 0%, #0f2818 50%, #1a3a28 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 20px;
  overflow-x: hidden;
  overflow-y: auto;
  position: relative;
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
  pointer-events: none;
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

/* 音乐控制按钮 */
.music-control {
  position: fixed;
  bottom: 40px;
  right: 40px;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.95);
  border: 2px solid #3a7d44;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #2d5016;
  transition: all 0.3s;
  z-index: 100;
}

.music-control:hover {
  transform: scale(1.1);
  box-shadow: 0 15px 40px rgba(58, 125, 68, 0.4);
}

/* 彩蛋 */
.easter-egg-trigger {
  position: fixed;
  top: 0;
  left: 0;
  width: 100px;
  height: 100px;
  cursor: pointer;
  z-index: 50;
  /* 👇 添加半透明提示 */
  background: rgba(58, 125, 68, 0.05);
  transition: all 0.3s;
}

.easter-egg-trigger:hover {
  background: rgba(58, 125, 68, 0.15);
}

.easter-egg {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(255, 255, 255, 0.98);
  padding: 50px;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  text-align: center;
  z-index: 200;
  animation: bounceIn 0.5s;
}

@keyframes bounceIn {
  0% {
    transform: translate(-50%, -50%) scale(0);
  }
  50% {
    transform: translate(-50%, -50%) scale(1.1);
  }
  100% {
    transform: translate(-50%, -50%) scale(1);
  }
}

.heart-icon {
  color: #3a7d44;
  animation: heartbeat 1s infinite;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

.easter-egg p {
  margin-top: 20px;
  font-size: 24px;
  color: #2d5016;
  font-weight: bold;
}

/* 信纸卡片（竹青色信纸） */
.letter-container {
  position: relative;
  z-index: 10;
  max-width: 900px;
  width: 100%;
  background: linear-gradient(180deg, #f5f8f3 0%, #edf5e8 50%, #f5f8f3 100%);
  border-radius: 8px;
  box-shadow: 
    0 25px 80px rgba(0, 0, 0, 0.4),
    inset 0 0 200px rgba(168, 216, 195, 0.1);
  padding: 0;
  animation: fadeInCard 1.2s ease-out;
  border: 3px solid #a8d8c3;
  overflow: hidden;
}

@keyframes fadeInCard {
  from {
    opacity: 0;
    transform: translateY(40px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

/* 信纸纹理 */
.paper-texture {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: 
    repeating-linear-gradient(
      0deg,
      transparent,
      transparent 29px,
      rgba(168, 216, 195, 0.03) 29px,
      rgba(168, 216, 195, 0.03) 30px
    );
  pointer-events: none;
  z-index: 1;
}

/* 顶部装饰 */
.letter-header {
  position: relative;
  height: 50px;
  background: linear-gradient(180deg, #a8d8c3 0%, #8bb896 100%);
  border-bottom: 2px solid #6d9d7d;
}

.header-decoration {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 15px;
  background: repeating-linear-gradient(
    90deg,
    #8bb896 0px,
    #a8d8c3 10px,
    #8bb896 20px
  );
}

.header-pattern {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.5), transparent);
}

/* 侧边装饰 */
.side-decoration {
  position: absolute;
  top: 80px;
  width: 30px;
  height: calc(100% - 160px);
  z-index: 2;
}

.side-decoration.left {
  left: 20px;
}

.side-decoration.right {
  right: 20px;
}

.deco-line {
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg, transparent, #a8d8c3, transparent);
  margin: 0 auto;
}

.deco-flower {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 25px;
  height: 25px;
  background: radial-gradient(circle, #7ecf8d, transparent 60%);
  border-radius: 50%;
  opacity: 0.4;
}

/* 主内容区 */
.letter-main {
  position: relative;
  z-index: 2;
  padding: 50px 80px 60px;
}

/* 标题 */
.letter-title {
  text-align: center;
  font-size: 42px;
  color: #2d5016;
  margin-bottom: 45px;
  font-weight: bold;
  letter-spacing: 12px;
  position: relative;
}

.title-char {
  display: inline-block;
  animation: titleReveal 0.6s ease-out backwards;
}

.title-char:nth-child(1) { animation-delay: 0.1s; }
.title-char:nth-child(2) { animation-delay: 0.2s; }
.title-char:nth-child(3) { animation-delay: 0.3s; }
.title-char:nth-child(4) { animation-delay: 0.4s; }
.title-char:nth-child(5) { animation-delay: 0.5s; }
.title-char:nth-child(6) { animation-delay: 0.6s; }
.title-char:nth-child(7) { animation-delay: 0.7s; }

@keyframes titleReveal {
  from {
    opacity: 0;
    transform: translateY(-20px) scale(0.8);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.letter-title::after {
  content: '';
  position: absolute;
  bottom: -15px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, transparent, #3a7d44, transparent);
  border-radius: 2px;
}

/* 信件内容 */
.letter-content {
  line-height: 2.2;
  font-size: 19px;
  color: #2d4a2d;
  margin-bottom: 30px;
  letter-spacing: 1px;
}

.letter-paragraph {
  margin-bottom: 30px;
  text-indent: 2em;
  opacity: 0;
  position: relative;
}

.letter-paragraph::first-letter {
  font-size: 24px;
  font-weight: bold;
  color: #3a7d44;
}

/* 五种文字展示效果 */
.letter-paragraph.fadeIn.is-visible {
  animation: textFadeIn 0.8s ease-out forwards;
}

@keyframes textFadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.letter-paragraph.slideIn.is-visible {
  animation: textSlideIn 0.8s ease-out forwards;
}

@keyframes textSlideIn {
  from {
    opacity: 0;
    transform: translateX(-50px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.letter-paragraph.scaleUp.is-visible {
  animation: textScaleUp 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
}

@keyframes textScaleUp {
  from {
    opacity: 0;
    transform: scale(0.5);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.letter-paragraph.flipIn.is-visible {
  animation: textFlipIn 0.8s ease-out forwards;
  transform-style: preserve-3d;
}

@keyframes textFlipIn {
  from {
    opacity: 0;
    transform: perspective(400px) rotateX(90deg);
  }
  to {
    opacity: 1;
    transform: perspective(400px) rotateX(0deg);
  }
}

.letter-paragraph.none {
  opacity: 1;
}

/* 签名 */
.letter-signature {
  text-align: right;
  margin-top: 50px;
  font-size: 20px;
  color: #3d5a3d;
  opacity: 0;
  transition: opacity 1s ease-out 0.5s;
  letter-spacing: 2px;
}

.letter-signature.is-visible {
  opacity: 1;
}

.signature-line {
  margin-bottom: 8px;
}

.signature-name {
  margin: 12px 0;
  font-size: 26px;
  font-weight: bold;
  color: #2d5016;
  letter-spacing: 4px;
}

.signature-date {
  font-size: 16px;
  color: #5a7d5a;
}

/* 底部装饰 */
.letter-footer {
  position: relative;
  height: 40px;
  background: linear-gradient(180deg, #8bb896 0%, #a8d8c3 100%);
  border-top: 2px solid #6d9d7d;
}

.footer-pattern {
  position: absolute;
  width: 100%;
  height: 100%;
  background: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 15px,
    rgba(255, 255, 255, 0.1) 15px,
    rgba(255, 255, 255, 0.1) 30px
  );
}

/* 过渡效果 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* 响应式 */
@media (max-width: 768px) {
  .letter-main {
    padding: 40px 40px 50px;
  }
  
  .letter-title {
    font-size: 32px;
    letter-spacing: 8px;
  }
  
  .letter-content {
    font-size: 17px;
  }
  
  .side-decoration {
    display: none;
  }
  
  .music-control {
    bottom: 20px;
    right: 20px;
    width: 50px;
    height: 50px;
  }
}
</style>