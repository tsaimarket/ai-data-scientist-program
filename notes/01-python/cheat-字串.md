---
name: cheat-字串
description: 字串索引/切片、常見字串函數、連結/重複/逃脫、字串格式化 format 與 f-string 速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M6,M10
---

# 字串速查（索引 / 函數 / 格式化）

## 建立

- 短字串：`'單'` 或 `"雙"` 引號。
- 長（多行）字串：一對三引號 `'''...'''`。

## 索引與切片

```python
s = 'Hello'
#    H  e  l  l  o
#    0  1  2  3  4      (從頭)
#   -5 -4 -3 -2 -1      (從尾)
s[0]        # 'H'
s[-1]       # 'o'
s[1:4]      # 'ell'   切片 s[start:end]（不含 end）
s[::2]      # 'Hlo'   s[start:end:interval]
s[::-1]     # 'olleH' 反轉
```

## 連結 / 重複 / 逃脫

| 操作 | 寫法 |
|---|---|
| 連結 | `'a' + 'b'` → `'ab'` |
| 重複 | `'ab' * 3` → `'ababab'` |
| 逃脫 | `\n`(換行) `\t`(tab) `\\`(反斜線) `\'`(引號) |

## 常見字串操作/函數

| 操作 | 說明 |
|---|---|
| `s[i]` / `s[i:j]` / `s[i:j:k]` | 索引 / 切片 / 帶間隔 |
| `s + s2`、`s*n` | 連結 / 重複 |
| `len(s)` | 長度 |
| `min(s)` / `max(s)` | 最小/最大字元 |
| `x in s` / `x not in s` | 子字串是否存在 |
| `s.upper()/.lower()` | 大小寫 |
| `s.strip()` | 去頭尾空白 |
| `s.split(',')` | 切成 list |
| `'-'.join(lst)` | list 合成字串 |
| `s.replace(a,b)` | 取代 |
| `s.find(x)` | 找位置（無回 -1） |

## 字串格式化（M10）

用 `{}` 標記要置換的位置。三種主流：

```python
a, b, c = 100, 8.9, 'hello'
# 1) str.format()
'{} {} {}'.format(a, b, c)
'{:10}'.format(c)      # 寬度10
'{:>10}'.format(c)     # 靠右；'<' 靠左；'^' 置中
'{:.2f}'.format(b)     # 浮點2位小數
'{:d}'.format(a)       # 整數
'{:#x}'.format(a)      # 加 0x 前綴（#b/#o 同理）
# 2) f-string（最推薦，Py3.6+）
f'{a} {b:.2f} {c:>10}'
# 3) % 舊式
'%d %.2f %s' % (a, b, c)
```

對齊/寬度/前綴口訣：`{:[對齊][寬度][.精度][型別]}`。

延伸：抓字串樣式用 [framework-正規表達式](framework-正規表達式.md)；字串其實是 immutable 序列，見 [cheat-資料型別總覽](cheat-資料型別總覽.md)。
