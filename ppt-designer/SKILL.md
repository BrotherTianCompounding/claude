---
name: ppt-designer
description: Use when given a PPT design document with one or more slide specs to generate a branded HTML presentation. Triggers on design documents with fields like 頁碼, 類型, 標題, or any structured multi-slide spec following the brand standard.
---

# PPT Designer（品牌簡報生成師）

## Overview

接收包含多頁說明的設計文件，按品牌規範生成單一自包含 HTML 簡報（16:9 / 1920×1080）。
可直接在瀏覽器中全螢幕播放，用左右鍵切換頁面，適合截圖作為 B-ROLL 素材。

---

## 品牌設計規範

### 尺寸與底色

| 項目 | 規格 |
|------|------|
| 畫布尺寸 | 1920 × 1080 px（16:9）|
| 背景色 | `#0a0a0a`（近純黑）|
| 正文色 | `#f0f0f0`（淺白）|
| 字體族 | `"Noto Sans TC", "PingFang TC", "Microsoft JhengHei", sans-serif` |

### 色彩系統

| 用途 | 色碼 |
|------|------|
| 主標題 | `#c8882a`（深沉琥珀金）|
| 次標題 / 說明 | `#f0f0f0` |
| 強調數字 / 金色高亮 | `#d4a844` |
| 上漲 / 正向 | `#5a8a6a`（低飽和金融綠）|
| 下跌 / 負向 | `#c05a4a`（低飽和磚紅）|
| 圖標描邊 | `#d0d0d0` |
| 裝飾線 | `#c8882a` |

### 字重分層

| 層級 | font-size | font-weight |
|------|-----------|-------------|
| 主標題 | 56–68px | 700 |
| 副標題 / 說明 | 28–36px | 400 |
| 超大數字強調 | 80–120px | 700 |
| 條列項目 | 30–34px | 400 |

### 圖標規範（SVG 線條藝術）

**必守原則：**
- `fill: none`，`stroke` 為主要描繪方式
- `stroke-width: 2`（一致，禁止混用粗細）
- `stroke-linecap: round`，`stroke-linejoin: round`
- 禁止實心填充、漸層、陰影（中心靶點除外，可用品牌色低飽和填充）

---

## SVG 圖標模板庫

### 1. 增長圖表 `chart-growth`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <line x1="12" y1="82" x2="88" y2="82"/>
  <line x1="12" y1="82" x2="12" y2="14"/>
  <polyline points="18,72 38,52 60,34 80,18" stroke="#5a8a6a"/>
  <circle cx="80" cy="18" r="3.5" fill="#5a8a6a" stroke="none"/>
</svg>
```

### 2. 錢袋 `money-bag`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M38,42 C22,48 18,62 18,70 C18,84 32,88 50,88 C68,88 82,84 82,70 C82,62 78,48 62,42 C58,32 50,26 50,26 C50,26 42,32 38,42 Z"/>
  <path d="M40,26 Q50,18 60,26"/>
  <text x="50" y="72" text-anchor="middle" font-size="22" fill="#d0d0d0" stroke="none" font-family="sans-serif" font-weight="400">$</text>
</svg>
```

### 3. 目標靶心 `target`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2">
  <circle cx="50" cy="50" r="42"/>
  <circle cx="50" cy="50" r="26"/>
  <circle cx="50" cy="50" r="9" fill="#5a8a6a" stroke="none"/>
</svg>
```

### 4. 鏈條連結 `chain-link`
```svg
<svg viewBox="0 0 100 100" fill="none" stroke="#d0d0d0" stroke-width="2.5" stroke-linecap="round">
  <rect x="12" y="38" width="36" height="24" rx="12"/>
  <rect x="52" y="38" width="36" height="24" rx="12"/>
</svg>
```

### 5. 團隊人物 `team`
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

## 設計文件輸入格式

每頁用以下欄位描述（未提及欄位可省略）：

```
頁碼：1
類型：標題頁
標題：主標題文字
副標題：（選填）補充說明
圖標：chart-growth
主體：正文段落或條列（多行用換行分隔）
強調數字：+32.5%
強調色：green（green / gold / red，預設 gold）
備註：版面提示
```

---

## 版面類型與 HTML 結構

### 標題頁 `標題頁`
```html
<section class="slide active">
  <div class="slide-inner center">
    <!-- 選填圖標 -->
    <div class="icon-wrap"><!-- SVG --></div>
    <h1 class="title">主標題</h1>
    <p class="subtitle">副標題</p>
  </div>
</section>
```

### 數據強調頁 `數據頁`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">頁面標題</h2>
    <div class="data-row">
      <div class="icon-wrap"><!-- SVG --></div>
      <span class="data-large gold">+32.5%</span>
    </div>
    <p class="body-text">說明文字</p>
  </div>
</section>
```

### 說明頁 `說明頁`
```html
<section class="slide">
  <div class="slide-inner">
    <div class="accent-bar"></div>
    <h2 class="title">頁面標題</h2>
    <p class="body-text">正文，<span class="gold">關鍵詞</span>用金色標記。</p>
  </div>
</section>
```

### 列表頁 `列表頁`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">頁面標題</h2>
    <ul class="list">
      <li>條列項目一</li>
      <li>條列項目二</li>
    </ul>
  </div>
</section>
```

### 圖示說明頁 `圖示頁`
```html
<section class="slide">
  <div class="slide-inner">
    <h2 class="title">頁面標題</h2>
    <div class="icon-grid">
      <div class="icon-item">
        <div class="icon-wrap"><!-- SVG --></div>
        <p class="icon-label">說明</p>
      </div>
    </div>
  </div>
</section>
```

---

## 完整 HTML 框架

輸出時以此為骨架，填入各頁 `<section>` 區塊：

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>簡報</title>
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

    <!-- ===== 在此插入各頁 <section class="slide"> ===== -->

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

## 執行流程

1. **解析** — 逐頁讀取設計文件欄位
2. **選型** — 依 `類型` 欄位套用對應版面結構
3. **圖標** — 按 `圖標` 欄位嵌入對應 SVG 模板
4. **色彩** — 強調數字依 `強調色` 欄位套用 `.gold` / `.green` / `.red`
5. **組裝** — 依序生成所有 `<section>` 填入 HTML 框架
6. **輸出** — 完整單一 `.html` 檔案，可直接瀏覽器開啟

---

## 常見問題處理

| 問題 | 處理方式 |
|------|---------|
| 標題文字過長 | 縮小至 48px，允許換行（line-height: 1.2）|
| 一頁多個圖標 | icon-grid 橫排，icon-wrap svg 縮至 80px |
| 純文字長段落 | 左側加 `accent-bar`，分段控制在 3–4 行 |
| 強調數字 + 圖標同頁 | data-row flex 排列，圖標左、數字右 |
| 無圖標的標題頁 | 省略 icon-wrap，標題垂直置中即可 |
