# PPT Designer（品牌简报生成师）

## Overview

接收包含多页说明的设计文件，按品牌规范生成单一自包含 HTML 简报（16:9 / 1920×1080）。
可直接在浏览器中全屏播放，用左右键切换页面，适合截图作为 B-ROLL 素材。

---

## 品牌设计规范

### 尺寸与底色

| 项目 | 规格 |
|------|------|
| 画布尺寸 | 1920 × 1080 px（16:9）|
| 背景色 | `#0a0a0a`（近纯黑）|
| 正文色 | `#f0f0f0`（浅白）|
| 字体族 | `"Noto Sans TC", "PingFang TC", "Microsoft JhengHei", sans-serif` |

### 色彩系统

| 用途 | 色码 |
|------|------|
| 主标题 | `#c8882a`（深沉琥珀金）|
| 次标题 / 说明 | `#f0f0f0` |
| 强调数字 / 金色高亮 | `#d4a844` |
| 上涨 / 正向 | `#5a8a6a`（低饱和金融绿）|
| 下跌 / 负向 | `#c05a4a`（低饱和砖红）|
| 图标描边 | `#d0d0d0` |
| 装饰线 | `#c8882a` |

### 字重分层

| 层级 | font-size | font-weight |
|------|-----------|-------------|
| 主标题 | 56–68px | 700 |
| 副标题 / 说明 | 28–36px | 400 |
| 超大数字强调 | 80–120px | 700 |
| 条列项目 | 30–34px | 400 |

### 图标规范（SVG 线条艺术）

**必守原则：**
- `fill: none`，`stroke` 为主要描绘方式
- `stroke-width: 2`（一致，禁止混用粗细）
- `stroke-linecap: round`，`stroke-linejoin: round`
- 禁止实心填充、渐层、阴影（中心靶点除外，可用品牌色低饱和填充）

---

## SVG 图标模板库

### 1. 增长图表 `chart-growth`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <line x1="12" y1="82" x2="88" y2="82"/>
  <line x1="12" y1="82" x2="12" y2="14"/>
  <polyline points="18,72 38,52 60,34 80,18" stroke="#5a8a6a"/>
  <circle cx="80" cy="18" r="3.5" fill="#5a8a6a" stroke="none"/>
</svg>
```

### 2. 钱袋 `money-bag`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M38,42 C22,48 18,62 18,70 C18,84 32,88 50,88 C68,88 82,84 82,70 C82,62 78,48 62,42 C58,32 50,26 50,26 C50,26 42,32 38,42 Z"/>
  <path d="M40,26 Q50,18 60,26"/>
  <text x="50" y="72" text-anchor="middle" font-size="22" fill="#d0d0d0" stroke="none" font-family="sans-serif" font-weight="400">$</text>
</svg>
```

### 3. 目标靶心 `target`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2">
  <circle cx="50" cy="50" r="42"/>
  <circle cx="50" cy="50" r="26"/>
  <circle cx="50" cy="50" r="9" fill="#5a8a6a" stroke="none"/>
</svg>
```

### 4. 链条连结 `chain-link`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2.5" stroke-linecap="round">
  <rect x="12" y="38" width="36" height="24" rx="12"/>
  <rect x="52" y="38" width="36" height="24" rx="12"/>
</svg>
```

### 5. 团队人物 `team`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <circle cx="50" cy="26" r="12"/>
  <path d="M26,82 Q30,58 50,54 Q70,58 74,82"/>
  <circle cx="20" cy="32" r="9"/>
  <path d="M4,82 Q8,64 20,61 Q30,64 33,72"/>
  <circle cx="80" cy="32" r="9"/>
  <path d="M67,72 Q70,64 80,61 Q92,64 96,82"/>
</svg>
```

---

## 设计文件输入格式

每页用以下栏位描述（未提及栏位可省略）：

```
页码：1
类型：标题页
标题：主标题文字
副标题：（选填）补充说明
图标：chart-growth
主体：正文段落或条列（多行用换行分隔）
强调数字：+32.5%
强调色：green（green / gold / red，预设 gold）
备注：版面提示
```

---

## 版面类型与 HTML 结构

### 标题页 `标题页`
```html
<section class="slide active">
  <div class="slide-inner center">
    <!-- 选填图标 -->
    <div class="icon-wrap"><!-- SVG --></div>
    <h1 class="title">主标题</h1>
    <p class="subtitle">副标题</p>
  </div>
</section>
```

### 数据强调页 `数据页`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">页面标题</h2>
    <div class="data-row">
      <div class="icon-wrap"><!-- SVG --></div>
      <span class="data-large gold">+32.5%</span>
    </div>
    <p class="body-text">说明文字</p>
  </div>
</section>
```

### 说明页 `说明页`
```html
<section class="slide">
  <div class="slide-inner">
    <div class="accent-bar"></div>
    <h2 class="title">页面标题</h2>
    <p class="body-text">正文，<span class="gold">关键词</span>用金色标记。</p>
  </div>
</section>
```

### 列表页 `列表页`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">页面标题</h2>
    <ul class="list">
      <li>条列项目一</li>
      <li>条列项目二</li>
    </ul>
  </div>
</section>
```

### 图示说明页 `图示页`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">页面标题</h2>
    <div class="icon-grid">
      <div class="icon-item">
        <div class="icon-wrap"><!-- SVG --></div>
        <p class="icon-label">说明</p>
      </div>
    </div>
  </div>
</section>
```

---

## 完整 HTML 框架

输出时以此为骨架，填入各页 `<section>` 区块：

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>简报</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { background: #000; overflow: hidden; font-family: "Noto Sans TC","PingFang TC","Microsoft JhengHei",sans-serif; }
.viewport { width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; }
.stage { width: 1920px; height: 1080px; position: relative; transform-origin: center center; }
.slide { width: 1920px; height: 1080px; background: #0a0a0a; position: absolute; top: 0; left: 0; display: none; padding: 80px 120px; }
.slide.active { display: flex; flex-direction: column; justify-content: center; }
.slide-inner { width: 100%; display: flex; flex-direction: column; gap: 32px; }
.slide-inner.center { align-items: center; text-align: center; }
.title { color: #c8882a; font-size: 62px; font-weight: 700; line-height: 1.25; }
.subtitle { color: #f0f0f0; font-size: 32px; font-weight: 400; }
.body-text { color: #e0e0e0; font-size: 30px; line-height: 1.9; }
.data-large { font-size: 100px; font-weight: 700; line-height: 1; }
.gold { color: #d4a844; }
.green { color: #5a8a6a; }
.red { color: #c05a4a; }
.data-row { display: flex; align-items: center; gap: 48px; }
.accent-bar { width: 6px; height: 100%; background: #c8882a; position: absolute; left: 0; top: 0; border-radius: 3px; }
.slide-inner { position: relative; padding-left: 0; }
.list { list-style: none; display: flex; flex-direction: column; gap: 20px; }
.list li { color: #e0e0e0; font-size: 32px; line-height: 1.6; padding-left: 32px; position: relative; }
.list li::before { content: "▸"; color: #d4a844; position: absolute; left: 0; }
.icon-wrap svg { width: 100px; height: 100px; }
.icon-grid { display: flex; gap: 60px; align-items: flex-start; }
.icon-item { display: flex; flex-direction: column; align-items: center; gap: 16px; flex: 1; }
.icon-label { color: #e0e0e0; font-size: 26px; text-align: center; line-height: 1.6; }
.page-num { position: absolute; bottom: 40px; right: 60px; color: #555; font-size: 22px; }
</style>
</head>
<body>
<div class="viewport">
  <div class="stage" id="stage">

    <!-- ===== 在此插入各页 <section class="slide"> ===== -->

  </div>
</div>
<script>
let cur = 0;
const slides = document.querySelectorAll('.slide');
function show(n) {
  slides[cur].classList.remove('active');
  cur = (n + slides.length) % slides.length;
  slides[cur].classList.add('active');
}
document.addEventListener('keydown', e => {
  if (e.key === 'ArrowRight' || e.key === ' ') show(cur + 1);
  if (e.key === 'ArrowLeft') show(cur - 1);
});
function scale() {
  const s = Math.min(window.innerWidth / 1920, window.innerHeight / 1080);
  document.getElementById('stage').style.transform = `scale(${s})`;
}
window.addEventListener('resize', scale); scale();
</script>
</body>
</html>
```

---

## 执行流程

1. **解析** — 逐页读取设计文件栏位
2. **选型** — 依 `类型` 栏位套用对应版面结构
3. **图标** — 按 `图标` 栏位嵌入对应 SVG 模板
4. **色彩** — 强调数字依 `强调色` 栏位套用 `.gold` / `.green` / `.red`
5. **组装** — 依序生成所有 `<section>` 填入 HTML 框架
6. **输出** — 完整单一 `.html` 档案，可直接浏览器开启

---

## 常见问题处理

| 问题 | 处理方式 |
|------|---------|
| 标题文字过长 | 缩小至 48px，允许换行（line-height: 1.2）|
| 一页多个图标 | icon-grid 横排，icon-wrap svg 缩至 80px |
| 纯文字长段落 | 左侧加 `accent-bar`，分段控制在 3–4 行 |
| 强调数字 + 图标同页 | data-row flex 排列，图标左、数字右 |
| 无图标的标题页 | 省略 icon-wrap，标题垂直置中即可 |
