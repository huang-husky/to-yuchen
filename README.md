# Vue 3 + Vite

# 升级版网页使用说明

## 🎉 主要改进

1. ✅ **星空粒子背景** - 密码页和所有页面都有漂亮的星空效果
2. ✅ **真实樱花花瓣** - 不再是方块，而是真实的樱花形状
3. ✅ **卷轴展开效果** - 古风卷轴替代了信封，配合古色古香的配色
4. ✅ **5种文字展示效果** - 可以自由选择喜欢的动画

---

## 📝 如何替换文件

### 方法1：手动替换（推荐）

在你的VSCode项目 `cy--` 里：

1. **替换 src/App.vue**
   * 用下载的 `App-v2.vue` 覆盖 `src/App.vue`
2. **替换 src/components/ 下的3个文件**
   * 用 `PasswordPage-v2.vue` 覆盖 `PasswordPage.vue`
   * 删除 `EnvelopePage.vue`
   * 添加 `ScrollPage.vue`（新的卷轴页面）
   * 用 `LetterPage-v2.vue` 覆盖 `LetterPage.vue`

### 最终文件结构

```
cy--/
├── src/
│   ├── components/
│   │   ├── PasswordPage.vue  (新版)
│   │   ├── ScrollPage.vue    (卷轴页面)
│   │   └── LetterPage.vue    (新版)
│   ├── App.vue  (更新的主文件)
│   └── main.js
└── ...
```

---

## 🎨 如何选择文字展示效果

打开 `LetterPage.vue`，找到第 **12 行**左右：

```javascript
const textAnimation = ref('fadeIn')  // 👈 改这里！
```

**可选值：**

1. **'fadeIn'** - 淡入效果（优雅渐现）✨ 推荐
2. **'slideIn'** - 滑入效果（从左侧滑入）
3. **'scaleUp'** - 缩放弹出（从小到大）
4. **'flipIn'** - 翻转进入（3D翻转效果）
5. **'none'** - 无动画（直接显示全部）

### 示例

想要滑入效果：

```javascript
const textAnimation = ref('slideIn')
```

想要3D翻转：

```javascript
const textAnimation = ref('flipIn')
```

想要立即显示所有内容：

```javascript
const textAnimation = ref('none')
```

---

## ✏️ 修改信件内容

在 `LetterPage.vue` 第 **23 行**左右：

```javascript
const paragraphsData = [
  '第一段内容...',
  '第二段内容...',
  '第三段内容...',
  // 继续添加
]
```

在 **152 行**左右修改签名：

```html
<div class="signature-name">小黄</div>  <!-- 改成你的名字 -->
<div>2024年12月</div>  <!-- 改成日期 -->
```

---

## 🎵 添加背景音乐（可选）

1. 准备一个 MP3 文件
2. 放到 `public/` 文件夹，命名为 `music.mp3`
3. 在 `LetterPage.vue` 找到第 **72-75 行**，取消注释：

```html
<!-- 删除这些注释符号 -->
<audio ref="audioElement" loop>
  <source src="/music.mp3" type="audio/mpeg">
</audio>
```

4. 找到第 **78-82 行**，取消注释音乐按钮：

```html
<!-- 删除这些注释符号 -->
<button class="music-control" @click="toggleMusic">
  <Volume2 v-if="musicPlaying" :size="24" />
  <VolumeX v-else :size="24" />
</button>
```

---

## 🚀 运行项目

```bash
npm run dev
```

然后 Ctrl + 点击链接，输入密码 `chenyu` 查看效果！

---

## 🎁 隐藏彩蛋

在信件页面，连续点击**左上角 3 次**会出现惊喜！🌸

---

## 💡 其他说明

* **密码**在 `PasswordPage.vue` 第 13 行修改
* **樱花数量**可以调整每个页面的 `v-for="n in 15"` 这个数字
* **星星数量**同样可以调整数字
* 所有颜色都可以在对应的 `<style>` 部分修改

祝你成功！如有问题随时问我！😊

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).
