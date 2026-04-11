# PPT Designer（品牌简报生成师）

## Overview

接收包含多页说明的设计文件，生成单一自包含 HTML 简报。
采用**滚动吸附**方式逐页浏览（鼠标滚轮/触控滑动），右侧有导航圆点可点击跳转。
适合在浏览器中全屏截图作为 B-ROLL 素材。

---

## 视觉风格规范

### 整体风格
- **浅色柔和背景**：每页使用不同的 Tailwind 浅色背景（如 `bg-pink-50`、`bg-teal-50`、`bg-amber-50`）
- **毛玻璃卡片**：内容区域使用 `glass-card` 样式（半透明白底 + backdrop-filter blur）
- **大号表情图标**：用 Emoji 作为视觉锚点（如 🌱💰、🎯、🚀、⚠️），替代 SVG 线条图标
- **圆润设计**：所有卡片使用大圆角（`rounded-3xl` / `rounded-2xl`）
- **进场动画**：fade-up、jelly弹跳、float悬浮、pulse心跳、wiggle摆动、rocket发射

### 关键尺寸
- **内容卡片**：`w-[92%] max-w-4xl`（占据屏幕大部分区域，减少四周空白）
- **内边距**：`p-10 md:p-14`（适中内边距，不浪费空间）
- **每页满屏**：`height: 100vh` + `scroll-snap-align: start`

### 色彩规则
- 正向/上涨：绿色系（`text-green-600`、`bg-green-50`、`border-green-200`）
- 负向/下跌：红色系（`text-red-500`、`bg-red-50`、`border-red-200`）
- 强调/重点：琥珀金色系（`text-amber-500`、`bg-amber-100`）
- 每页主色调通过背景色区分，卡片内部保持中性

### 字体
- 主字体：`'Nunito', sans-serif`（通过 Google Fonts 引入）
- 中文回退：系统默认
- 标题：`font-black`（900 weight）
- 正文：`font-bold`（700）或默认（400）

---

## 技术栈

- **Tailwind CSS**：通过 CDN `<script src="https://cdn.tailwindcss.com"></script>` 引入
- **Google Fonts**：Nunito（wght 400;700;900）
- **FontAwesome**：备用图标（大部分场景用 Emoji 即可）
- **纯原生 JS**：IntersectionObserver 驱动动画 + 导航圆点

---

## HTML 骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>简报标题</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&display=swap" rel="stylesheet">
<style>
body{font-family:'Nunito',sans-serif;background:#f8fafc;margin:0;padding:0}
::-webkit-scrollbar{width:0;background:transparent}
.scroll-container{scroll-snap-type:y mandatory;overflow-y:scroll;height:100vh;scroll-behavior:smooth}
.scroll-section{scroll-snap-align:start;height:100vh;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden}
.glass-card{background:rgba(255,255,255,0.65);backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);border:2px solid rgba(255,255,255,0.8);box-shadow:0 20px 40px rgba(0,0,0,0.08);border-radius:2.5rem}
/* 动画 */
.anim-fade-up{opacity:0;transform:translateY(50px) scale(0.95);transition:all 0.8s cubic-bezier(0.34,1.56,0.64,1)}
.is-visible .anim-fade-up{opacity:1;transform:translateY(0) scale(1)}
.delay-100{transition-delay:100ms}.delay-200{transition-delay:200ms}.delay-300{transition-delay:300ms}.delay-400{transition-delay:400ms}.delay-500{transition-delay:500ms}
@keyframes jelly{0%{transform:scale(1,1)}30%{transform:scale(1.25,0.75)}40%{transform:scale(0.75,1.25)}50%{transform:scale(1.15,0.85)}65%{transform:scale(0.95,1.05)}75%{transform:scale(1.05,0.95)}100%{transform:scale(1,1)}}
.is-visible .anim-jelly{animation:jelly 1.2s ease-in-out forwards}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-15px)}}
.is-visible .anim-float{animation:float 3s ease-in-out infinite}
@keyframes pulse-soft{0%,100%{transform:scale(1);box-shadow:0 0 0 0 rgba(255,255,255,0.7)}50%{transform:scale(1.05);box-shadow:0 0 0 20px rgba(255,255,255,0)}}
.is-visible .anim-pulse{animation:pulse-soft 2s infinite;border-radius:50%}
@keyframes wiggle{0%,100%{transform:rotate(-5deg)}50%{transform:rotate(5deg)}}
.is-visible .anim-wiggle{animation:wiggle 2s ease-in-out infinite}
@keyframes rocket-launch{0%{transform:translate(-80px,80px) scale(0.5);opacity:0}50%{transform:translate(10px,-10px) scale(1.1);opacity:1}100%{transform:translate(0,0) scale(1);opacity:1}}
.rocket-hidden{opacity:0}
.is-visible .anim-rocket{animation:rocket-launch 1.2s cubic-bezier(0.175,0.885,0.32,1.275) forwards}
/* 导航圆点 */
.nav-dot{width:10px;height:10px;border-radius:50%;background-color:rgba(0,0,0,0.15);transition:all 0.3s ease;cursor:pointer}
.nav-dot.active{background-color:#ec4899;transform:scale(1.6);height:20px;border-radius:10px}
/* 装饰圆 */
.deco-circle{position:absolute;border-radius:50%;background:rgba(255,255,255,0.4);backdrop-filter:blur(8px);z-index:0;pointer-events:none}
</style>
</head>
<body class="text-slate-700 antialiased selection:bg-pink-300 selection:text-white">

<!-- 右侧导航圆点 -->
<div class="fixed right-6 top-1/2 transform -translate-y-1/2 flex flex-col gap-3 z-50" id="nav-dots"></div>

<!-- 滚动主容器 -->
<div class="scroll-container" id="main-scroll">

  <!-- ===== 在此插入各页 <section class="scroll-section"> ===== -->

</div>

<script>
document.addEventListener("DOMContentLoaded",()=>{
  const sections=document.querySelectorAll('.scroll-section');
  const navContainer=document.getElementById('nav-dots');
  sections.forEach((_,i)=>{
    const dot=document.createElement('div');
    dot.classList.add('nav-dot');
    if(i===0)dot.classList.add('active');
    dot.dataset.index=i;
    dot.addEventListener('click',()=>{sections[i].scrollIntoView({behavior:'smooth'})});
    navContainer.appendChild(dot);
  });
  const navDots=document.querySelectorAll('.nav-dot');
  const observer=new IntersectionObserver(entries=>{
    entries.forEach(entry=>{
      const idx=entry.target.getAttribute('data-index');
      if(entry.isIntersecting){
        entry.target.classList.add('is-visible');
        navDots.forEach(d=>d.classList.remove('active'));
        navDots[idx].classList.add('active');
      }else{entry.target.classList.remove('is-visible')}
    });
  },{root:document.getElementById('main-scroll'),threshold:0.5});
  sections.forEach(s=>observer.observe(s));
  setTimeout(()=>{sections[0].classList.add('is-visible')},100);
});
</script>
</body>
</html>
```

---

## 每页结构模板

每一页都是一个 `<section class="scroll-section">` 包含一个 `glass-card`：

```html
<section class="scroll-section bg-[颜色]-50" data-index="[页码从0开始]">
  <div class="glass-card w-[92%] max-w-4xl p-10 md:p-14 text-center relative z-10">
    <!-- Emoji 图标 -->
    <div class="text-7xl mb-6 anim-fade-up anim-jelly drop-shadow-lg">[表情]</div>
    <!-- 标题 -->
    <h2 class="text-3xl md:text-4xl font-black mb-6 text-[颜色]-600 anim-fade-up delay-100">标题</h2>
    <!-- 正文内容区 -->
    <div class="anim-fade-up delay-200">
      <!-- 根据内容类型选择：列表、卡片网格、流程箭头、对比双列等 -->
    </div>
  </div>
</section>
```

### 常用内容组件

**对比双列（如选股vs系统、牛vs熊）：**
```html
<div class="grid grid-cols-1 md:grid-cols-2 gap-5">
  <div class="bg-red-50 p-6 rounded-3xl border-2 border-red-200">...</div>
  <div class="bg-green-50 p-6 rounded-3xl border-2 border-green-200">...</div>
</div>
```

**流程箭头（如路线图、Wheel循环）：**
```html
<div class="flex items-center justify-center gap-2 flex-wrap">
  <span class="bg-green-100 text-green-700 px-4 py-3 rounded-2xl font-black text-sm">节点1</span>
  <span class="text-xl text-slate-300">→</span>
  <span class="bg-amber-100 text-amber-700 px-4 py-3 rounded-2xl font-black text-sm">节点2</span>
</div>
```

**数据卡片网格（如仓位配比、核心指标）：**
```html
<div class="grid grid-cols-2 md:grid-cols-4 gap-4">
  <div class="bg-blue-100 p-5 rounded-2xl text-center border-2 border-blue-300">
    <p class="font-black text-blue-600 text-3xl">50%</p>
    <p class="font-black text-slate-700 mt-1">标签</p>
  </div>
</div>
```

**步骤列表（如开仓三步曲）：**
```html
<div class="flex items-center gap-4 bg-white/90 p-5 rounded-3xl shadow-sm border border-white">
  <div class="w-12 h-12 flex-shrink-0 bg-amber-100 rounded-full flex items-center justify-center text-amber-600 text-xl font-black">1</div>
  <div class="flex-1">
    <p class="font-black text-slate-800 text-lg">步骤标题</p>
    <p class="text-sm text-slate-500 mt-1">步骤说明</p>
  </div>
  <div class="text-3xl text-green-400">✓</div>
</div>
```

**强调金句：**
```html
<p class="text-amber-500 font-black mt-6 text-xl">金句内容 💪</p>
```

---

## 动画选择指南

| 内容类型 | 推荐动画 |
|---------|---------|
| 标题页 Emoji | `anim-jelly`（弹跳出场） |
| 数据/成就展示 | `anim-pulse`（心跳强调） |
| 阶段里程碑 | `anim-rocket`（火箭发射） |
| 常驻展示 | `anim-float`（悬浮呼吸） |
| 警告/风控 | `anim-wiggle`（摆动提醒） |
| 正文内容 | `anim-fade-up` + `delay-N00`（逐层淡入） |

---

## 背景色轮换建议

页面之间的背景色不要重复太近，建议按以下顺序轮换：
`pink-50` → `sky-50` → `teal-50` → `violet-50` → `blue-50` → `emerald-50` → `purple-100` → `fuchsia-50` → `amber-50` → `slate-100` → `orange-50` → `indigo-50` → `rose-50` → `cyan-50` → `green-50` → `stone-100`

---

## 执行流程

1. **解析** — 逐页读取设计文件内容
2. **选色** — 为每页分配不同的浅色背景
3. **选 Emoji** — 为每页选择 1-2 个与内容匹配的大号表情图标
4. **选动画** — 根据内容类型分配进场动画
5. **选组件** — 根据内容结构选择双列对比/流程箭头/数据网格/步骤列表等
6. **组装** — 依序生成所有 `<section>` 填入 HTML 骨架
7. **输出** — 完整单一 `.html` 文件，浏览器打开即可滚动浏览

---

## 注意事项

| 问题 | 处理方式 |
|------|---------|
| 内容卡片太小四周空白多 | 使用 `w-[92%] max-w-4xl`，不要用 `max-w-2xl` |
| 标题文字过长 | 缩小至 `text-2xl`，允许换行 |
| 一页内容太多 | 拆分成多页，保持每页聚焦一个概念 |
| 需要强调对比 | 用 `grid-cols-2` 双列卡片，颜色对立 |
| 需要展示流程 | 用 flex 横排 + 箭头 `→` 连接 |
