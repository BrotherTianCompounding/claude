# Remotion 动画美工

## Overview

接收设计文档，输出可直接运行的 Remotion 动画组件代码。目标：**零二次修改，上传即用**。

---

## 执行流程

1. **精读设计文档** — 提取：时长、画面元素、动效时序、配色、文案
2. **换算帧数** — 默认 30fps，时长(秒) × 30 = 总帧数
3. **规划时序** — 把动效按先后排列，分配每个元素的入场帧
4. **生成代码** — 输出完整可运行的 `.tsx` 文件
5. **附说明** — 注明如何集成到 `Root.tsx`

---

## Remotion 核心 API 速查

### 基础钩子
```tsx
const frame = useCurrentFrame();           // 当前帧（0开始）
const { fps, durationInFrames, width, height } = useVideoConfig();
```

### 动画函数
```tsx
// spring：物理弹簧，适合入场/出场
const progress = spring({
  frame,
  fps,
  from: 0, to: 1,
  config: { damping: 12, stiffness: 100, mass: 1 },
  delay: 10,          // 延迟帧数
  durationInFrames: 30,
});

// interpolate：线性插值，适合颜色/透明度/位移
const opacity = interpolate(frame, [0, 20], [0, 1], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});

// interpolateColors
import { interpolateColors } from 'remotion';
const color = interpolateColors(frame, [0, 30], ['#ff0000', '#00ff00']);
```

### 布局组件
```tsx
<AbsoluteFill style={{ backgroundColor: '#0f172a' }}>
  {/* 全屏容器，子元素用 position:absolute 定位 */}
</AbsoluteFill>

<Sequence from={30} durationInFrames={60}>
  {/* from帧开始显示，持续60帧 */}
</Sequence>

<Series>
  <Series.Sequence durationInFrames={30}><A /></Series.Sequence>
  <Series.Sequence durationInFrames={30}><B /></Series.Sequence>
</Series>
```

### Easing 缓动
```tsx
import { Easing, interpolate } from 'remotion';
interpolate(frame, [0, 30], [0, 1], {
  easing: Easing.out(Easing.cubic),
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

---

## 标准动效模式

### 淡入上移（文字入场首选）
```tsx
const enter = interpolate(frame, [startFrame, startFrame + 20], [0, 1], {
  extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
  easing: Easing.out(Easing.cubic),
});
const style = {
  opacity: enter,
  transform: `translateY(${interpolate(enter, [0, 1], [30, 0])}px)`,
};
```

### 数字滚动（计数器效果）
```tsx
const value = Math.round(interpolate(frame, [startFrame, startFrame + 45], [0, targetValue], {
  extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
  easing: Easing.out(Easing.expo),
}));
// 渲染：{value.toLocaleString()}
```

### 进度条
```tsx
const width = interpolate(frame, [startFrame, startFrame + 40], [0, 100], {
  extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
});
// style={{ width: `${width}%` }}
```

### 高亮闪烁（强调某元素）
```tsx
const highlight = Math.sin((frame - startFrame) * 0.3) * 0.5 + 0.5; // 0~1循环
// 用于 boxShadow 强度或 opacity 脉冲
```

---

## 代码结构模板

```tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, spring, interpolate, Easing, Sequence } from 'remotion';

// ── 设计 Token ──────────────────────────────────────────────
const COLORS = {
  bg: '#0f172a',
  accent: '#f59e0b',
  text: '#f1f5f9',
  muted: '#94a3b8',
};
const FONT = { heading: 'bold 48px "Inter", sans-serif', body: '24px "Inter", sans-serif' };

// ── 子组件 ──────────────────────────────────────────────────
const Title: React.FC<{ frame: number }> = ({ frame }) => {
  const opacity = interpolate(frame, [0, 20], [0, 1], { extrapolateLeft: 'clamp', extrapolateRight: 'clamp' });
  const y = interpolate(opacity, [0, 1], [40, 0]);
  return (
    <div style={{ position: 'absolute', top: 80, left: 80, opacity, transform: `translateY(${y}px)` }}>
      <div style={{ color: COLORS.text, font: FONT.heading }}>标题文字</div>
    </div>
  );
};

// ── 主组件 ──────────────────────────────────────────────────
export const MyAnimation: React.FC = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  return (
    <AbsoluteFill style={{ backgroundColor: COLORS.bg, fontFamily: 'Inter, sans-serif' }}>
      <Title frame={frame} />
      {/* 更多元素 */}
    </AbsoluteFill>
  );
};
```

### Root.tsx 注册方式
```tsx
// 在 Root.tsx 的 <Composition> 里添加：
<Composition
  id="MyAnimation"
  component={MyAnimation}
  durationInFrames={90}   // 时长(秒) × fps
  fps={30}
  width={1920}
  height={1080}
/>
```

---

## 视频规格默认值

| 参数 | 默认值 | 说明 |
|------|--------|------|
| fps | 30 | 帧率 |
| width | 1920 | 像素 |
| height | 1080 | 像素 |
| 背景色 | `#0f172a` | 深蓝黑，与频道VI一致 |
| 主强调色 | `#f59e0b` | 琥珀黄 |
| 字体 | Inter / 思源黑体 | 中英文混排 |

> 设计文档有明确规格时，以文档为准覆盖以上默认值。

---

## 常见错误

| 错误 | 修正 |
|------|------|
| `interpolate` 超出范围报错 | 始终加 `extrapolateLeft/Right: 'clamp'` |
| 动画一闪而过 | 检查帧范围，`durationInFrames` 是否太小 |
| 文字模糊 | 用 `fontSize` 数字而非 `font` 简写；避免非整数 transform |
| spring 不停抖动 | 调高 `damping`（推荐 12~20），降低 `stiffness` |
| 元素在错误时间出现 | 用 `<Sequence from={n}>` 包裹，而非纯 interpolate 控制 |

---

## 输出规范

每次输出必须包含：
1. **完整 `.tsx` 文件内容** — 可直接复制粘贴
2. **文件命名建议** — 如 `RollMechanism.tsx`
3. **Root.tsx 注册代码片段** — `<Composition>` 那几行
4. **时序说明表**（可选）— 列出每个元素的入场帧，方便微调
