# SRT Chinese-to-English Translator

Translate a Chinese SRT subtitle file into English, preserving all timestamps, sequence numbers, and formatting. Output a YouTube-ready English SRT file.

## Workflow

1. Read the full Chinese SRT file
2. Translate each subtitle entry to English
3. Write the English SRT to the same directory, filename with `-EN` suffix
4. Report completion stats

## Translation Rules

### Preserve as-is (NEVER translate these)
- **Options terms:** Sell Put, Buy Call, Covered Call, Iron Condor, LEAPS, Wheel Strategy, Synthetic Long, Credit Spread, Cash-Secured Put
- **Greeks:** Delta, Theta, Gamma, Vega, IV
- **Trading terms:** premium, strike price, expiration, margin, roll, OTM, ITM, ATM, at-the-money, out-of-the-money
- **Tickers:** QQQ, SPY, TQQQ, NVDA, AAPL, MSFT, SOXL, SOFI, COHERENT, MU, APP
- **People/brands:** Tom Lee, Dan Ives, Fundstrat, Wedbush, Investing.com, SpaceX

### Translation style
- Meaning-based, NOT word-for-word
- Conversational YouTube tone — casual, direct, like talking to friends
- US format for money: $400K, $30K, $1,570, 15%
- Keep the speaker's personality and energy
- Max 42 characters per line (YouTube readability)

### Common translations
| Chinese | English |
|---------|---------|
| 权利金 | premium |
| 行权价 | strike price |
| 行权 / 被行权 | assignment / get assigned |
| 浮亏 | unrealized loss |
| 割肉 | panic sell / cut losses |
| 抄底 | buy the dip |
| 收租 | collect rent / income |
| 展期 / ROLL | roll |
| 正股 | the underlying stock / shares |
| 黑天鹅 | black swan |
| 安全垫 | safety cushion / margin of safety |
| 隐含波动率 | implied volatility |
| 公允价值 | fair value |

## Output Format

- **Encoding:** UTF-8 with BOM (YouTube recommended)
- **Filename:** Same as input, add `-EN` before `.srt` (e.g., `字幕SRT.srt` → `字幕SRT-EN.srt`)
- **Location:** Same directory as input file
- **Format:** Standard SRT — timestamps unchanged, sequence numbers unchanged

## Quality Checklist (before saving)

1. Sequence numbers: continuous from 1, match original count
2. Timestamps: identical to Chinese version, no changes
3. No untranslated Chinese text remaining
4. Options/financial terms preserved in English
5. Tickers not translated
6. UTF-8 BOM encoding

## Completion Report

```
## Translation Complete

- Subtitle entries: XX (matches original)
- Output file: [path]
- Encoding: UTF-8 with BOM

All timestamps preserved. Ready for YouTube upload.
```
