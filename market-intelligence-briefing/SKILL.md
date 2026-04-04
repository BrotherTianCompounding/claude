---
name: market-intelligence-briefing
description: Use when the user wants the latest market outlook, predictions, or forward-looking views from key institutions and influencers including CNBC, Tom Lee, Dan Ives, White House, Federal Reserve, and Elon Musk. Triggers on phrases like "最新市场观点", "market briefing", "谁在看涨/看跌", "大佬怎么说", "机构预判", "market predictions".
---

# Market Intelligence Briefing

## Overview

Search, aggregate, and quote the latest forward-looking market views from a fixed set of authoritative sources. Every claim must be attributed with a direct quote or close paraphrase, the speaker's name, and the date.

## Sources to Cover (Always All 6)

| Source | Search Strategy |
|--------|----------------|
| **CNBC** | Latest analyst segments, market outlook articles |
| **Tom Lee** (Fundstrat) | Recent interviews, tweets, price targets |
| **Dan Ives** (Wedbush) | Tech sector calls, price target changes |
| **White House** | Economic policy statements, tariff/trade signals |
| **Federal Reserve** | Fed Chair / FOMC member speeches, dot plot commentary |
| **Elon Musk** | X posts on markets, economy, DOGE fiscal impact |

## Search Protocol

Run **one WebSearch per source** in parallel, using recent time-scoped queries:

```
"Tom Lee" market outlook 2025
"Dan Ives" stock forecast site:cnbc.com OR site:wedbush.com
CNBC market outlook this week
White House economic statement tariffs 2025
Federal Reserve Jerome Powell speech 2025
Elon Musk market economy X post 2025
```

Always add the current year or "this week" to bias toward recency.

## Output Format

Present results as a structured briefing:

```
## 市场前瞻简报 — [日期]

### 1. 美联储 (Federal Reserve)
> "[直接引语或核心表态]"
— Jerome Powell / [Fed官员姓名], [日期], [来源链接或出处]

核心观点：[1-2句总结]

### 2. 白宫 (White House)
> "[直接引语]"
— [发言人/官员], [日期], [来源]

核心观点：...

### 3. CNBC
> "[主播/分析师原话]"
— [姓名], CNBC, [日期]

核心观点：...

### 4. Tom Lee (Fundstrat)
> "[原话]"
— Tom Lee, [日期], [来源]

核心观点：...

### 5. Dan Ives (Wedbush)
> "[原话]"
— Dan Ives, Wedbush, [日期], [来源]

核心观点：...

### 6. Elon Musk
> "[X帖子原文或采访原话]"
— Elon Musk, [日期], [来源: X/@elonmusk 或媒体]

核心观点：...

---
### 综合研判
[3-5句话综合所有观点，指出共识与分歧]
```

## Rules

- **每条必须有归属** — 不允许无来源的"有分析师认为"
- **优先直接引语** — 找不到原话时用"据[来源]报道，[姓名]表示..."
- **标注日期** — 每条观点必须注明发表日期（精确到周）
- **标注分歧** — 若观点互相矛盾，在综合研判中明确指出
- **不预测** — 只汇报他人观点，不添加自己的市场判断

## Common Mistakes

| 错误 | 正确做法 |
|------|---------|
| 混合多人观点不标出处 | 每段单独归属 |
| 只搜索部分来源 | 必须覆盖全部6个来源 |
| 引用几个月前的旧观点 | 搜索时加年份/月份过滤 |
| 找不到内容就跳过 | 注明"本次搜索未找到[来源]近期表态" |
