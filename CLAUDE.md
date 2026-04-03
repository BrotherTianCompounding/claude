# 天哥 YouTube 内容自动化系统 — Claude 工作指南

## 项目背景
这是天哥的 YouTube 频道内容生产自动化系统。
- 频道方向：期权交易、长线投资策略教学（中文财经）
- 更新频率：每周一期，约15分钟
- 目标：自动化选题、标题、脚本、B-roll 清单的生成

## 文件结构
```
youtube/
├── CLAUDE.md              # 本文件，Claude 每次都要先读
├── style_guide/
│   └── tiange_style_guide.md   # 天哥风格指南（必读）
├── templates/
│   └── script_template.md      # 脚本生成模板（必读）
├── feedback/
│   └── feedback_log.md         # 历史反馈记录（必读）
├── scripts/                    # 历史脚本存档
├── topics/                     # 话题研究
└── output/                     # 每周生成的内容输出
```

## 每次生成脚本前，必须按顺序读取：
1. `style_guide/tiange_style_guide.md` — 了解天哥的风格
2. `templates/script_template.md` — 使用正确的结构
3. `feedback/feedback_log.md` — 应用历史反馈
4. `topics/` 目录下的话题研究（如果有）

## 核心规则

### 语言
- 主体中文，专业期权术语保留英文（Delta, Theta, Sell Put, LEAPS 等）
- 不要翻译专有名词（QQQ, SPY, TQQQ 等）

### 结构（必须遵守）
- Hook → 自我介绍 → 免责声明 → PASTOR正文 → VIP Discord → 固定片尾语
- 正文用`【第X部分】`标注
- B-roll 用`[📈 OptionStrat...]`或`[🖥️ Canvas...]`标注

### 内容质量
- 必须包含具体数字（价格、回报率、胜率等）
- 必须有生活化类比
- 必须有风险提示
- 下期预告用悬念式提问结尾

### 篇幅
- 目标约2000字（对应约15分钟视频）
- 不要过度简化，细节是天哥内容的核心价值

## 反馈循环
当天哥对生成内容提出修改意见时：
1. 按意见修改脚本
2. 将反馈要点记录到 `feedback/feedback_log.md`
3. 下次生成时自动应用

## 话题搜索方向
- YouTube：中文财经频道的热门视频（期权/ETF/退休规划相关）
- Reddit：r/options, r/investing, r/ChineseInvestment
- 市场事件：VIX波动、财报季、美联储会议等
- 策略进阶：上期讲了X，这期可以讲X的进阶/延伸

## 当前 Discord VIP 人数（需定期更新）
> 最后已知：200多人（截至2026年3月）
> 账户规模：约40万美金（截至第36期记录）
