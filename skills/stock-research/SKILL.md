---
name: stock-research
description: Use when the user provides a stock ticker (e.g. AAPL, TSLA, NVDA, QQQ) and wants to know recent price action, hot news, and why the stock is going up or down. Triggers on stock symbols, "为什么涨", "为什么跌", "stock news", "最近行情", "个股分析".
---

# Stock Research — 个股热点速查

## Overview

输入一个股票代码，自动搜索该股票的最新新闻热点和近期股价走势，分析涨跌的主要原因。

## Input

用户提供股票代码，例如：`NVDA`、`AAPL`、`TSLA`、`QQQ`

## Search Protocol

收到股票代码后，**并行**执行以下 4 组 WebSearch：

| # | 搜索内容 | 搜索词模板 |
|---|---------|-----------|
| 1 | 最新新闻 | `"{TICKER}" stock news this week {YEAR}` |
| 2 | 股价走势 | `"{TICKER}" stock price today performance` |
| 3 | 分析师观点 | `"{TICKER}" analyst rating upgrade downgrade {YEAR}` |
| 4 | 中文热点（可选） | `"{TICKER}" 股票 最新消息 {YEAR}` |

- `{TICKER}` = 用户输入的股票代码
- `{YEAR}` = 当前年份，确保搜索结果时效性
- 如果是 ETF（如 QQQ, SPY, VOO），第3组改为搜索资金流向：`"{TICKER}" ETF fund flow inflow outflow {YEAR}`

## 补充数据（可选）

如果初始搜索信息不足，可追加：

- 财报相关：`"{TICKER}" earnings results {YEAR}"`
- 行业动态：`"{TICKER}" sector industry trend {YEAR}"`
- 期权异动：`"{TICKER}" options unusual activity {YEAR}"`

## Output Format

```markdown
# {TICKER} — 个股速查报告
> 生成日期：{DATE}

## 近期股价表现
- 当前价格：$XXX.XX
- 近1周涨跌：+X.X% / -X.X%
- 近1月涨跌：+X.X% / -X.X%
- 52周范围：$XXX — $XXX（如能找到）

## 最新热点新闻
1. **[新闻标题1]** — [来源], [日期]
   > [1-2句关键内容摘要]
2. **[新闻标题2]** — [来源], [日期]
   > [1-2句关键内容摘要]
3. **[新闻标题3]** — [来源], [日期]
   > [1-2句关键内容摘要]
（列出3-5条最重要的新闻）

## 分析师/市场观点
- [分析师/机构]：[评级/目标价/核心观点] — [日期]
- [分析师/机构]：[评级/目标价/核心观点] — [日期]

## 涨跌原因分析

### 主要驱动因素
1. **[因素1]**：[具体说明，引用数据]
2. **[因素2]**：[具体说明，引用数据]
3. **[因素3]**：[具体说明，引用数据]

### 风险/利空因素
- [风险1]
- [风险2]

## 一句话总结
[用一句通俗的话概括：这只股票最近为什么涨/跌]
```

## Rules

- **必须标注来源和日期** — 每条新闻、每个数据点都要有出处
- **优先时效性** — 只关注最近1-2周的新闻，过时信息标注日期
- **数据要具体** — 不说"大幅上涨"，说"+12.3%"
- **区分事实与观点** — 新闻事实和分析师预测要分开
- **找不到就说** — 如果某项数据搜不到，注明"未找到"而非编造
- **涨跌分析要有逻辑链** — 不能只列新闻，要解释新闻→股价的因果关系

## Common Mistakes

| 错误 | 正确做法 |
|------|---------|
| 只列新闻不分析原因 | 必须有"涨跌原因分析"板块，解释因果 |
| 数据不标日期 | 股价数据注明截至日期 |
| 混淆历史和当前 | 明确区分"近期"和"历史"信息 |
| 编造具体数字 | 搜不到就写"未找到最新数据" |
| 忽略利空只讲利好 | 必须同时列出风险因素 |
