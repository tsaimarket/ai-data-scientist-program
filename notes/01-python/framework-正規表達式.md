---
name: framework-正規表達式
description: 正規表達式 regex 規則表（量詞/字元類/錨點/\d\w\s）+ re 模組 match/search/findall 速查
metadata:
  type: framework
book: 快速闖關Python語法世界
chapter: M10
---

# 正規表達式 regex + re 模組速查

regex＝用一套規則抓符合的字串。線上測試工具：pythex.org。

## 量詞 / 字元類 / 錨點

| 符號 | 意義 |
|---|---|
| `?` | 0 或 1 個 |
| `*` | 0 或多個 |
| `+` | 至少 1 個 |
| `{N}` | 剛好 N 個 |
| `{N,}` | 至少 N 個 |
| `{N,M}` | N 到 M 個 |
| `.` | 任一字元 |
| `\` | 逃脫字元 |
| `[...]` | 字元集合，集合內任一 |
| `[^...]` | 排除集合內字元 |
| `^...` | 以...開頭 |
| `...$` | 以...結尾 |

## 預定義字元類

| 符號 | 等同 | 意義 |
|---|---|---|
| `\d` | `[0-9]` | 數字 |
| `\D` | `[^0-9]` | 非數字 |
| `\w` | `[a-zA-Z0-9_]` | 字母數字底線 |
| `\W` | `[^a-zA-Z0-9_]` | 非 \w |
| `\s` | `[\f\n\r\t\v ]` | 空白字元 |
| `\S` | 非空白 | |
| `\n \t \r \f \v` | | 換行/tab/歸位/換頁/垂直tab |

## re 模組三大函數

| 函數 | 行為 | 回傳 |
|---|---|---|
| `re.match(p, s)` | **從第一個字元起**比對 | match 物件 / None |
| `re.search(p, s)` | 搜尋字串中**任何位置**第一個符合 | match 物件 / None |
| `re.findall(p, s)` | 找出**所有**符合 | list |

```python
import re
re.match(r'\d+', '123abc')        # 匹配開頭 → <match '123'>
re.match(r'\d+', 'abc123')        # 開頭非數字 → None
re.search(r'\d+', 'abc123')       # 任意位置 → <match '123'>
re.findall(r'\d+', 'a1b22c333')   # ['1','22','333']

m = re.search(r'(\d+)-(\d+)', '12-34')
m.group()    # '12-34'   m.group(1) → '12'   m.group(2) → '34'
```

⚠️ pattern 用 **raw string** `r'...'` 避免反斜線被吃掉。

實務對接：抓違規話術/格式校驗 → 與 vault 的 （規則式質檢）同源思路。字串本身操作見 [cheat-字串](cheat-字串.md)。
