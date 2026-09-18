---
name: cheat-例外處理與檔案io
description: try/except/finally、raise/assert 例外處理，CSV 格式，open 讀寫檔案速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M16
---

# 例外處理與檔案 IO 速查

## 例外處理 try / except

程式執行時可能出現預期外行為，用 try/except 攔截：

```python
try:
    x = int(input())
    y = 10 / x
except ZeroDivisionError:
    print('不能除以零')
except ValueError:
    print('不是數字')
except Exception as e:    # 萬用攔截
    print('其他錯誤', e)
else:
    print('沒出錯才跑')
finally:
    print('一定會跑（收尾/關檔）')
```

## raise（人工製造例外）

```python
raise ValueError('自訂錯誤訊息')
```

## assert（檢查假設）

```python
assert <condition>, <錯誤訊息>   # 訊息可省略
assert age >= 0, 'age 不可為負'  # 不滿足 → 拋 AssertionError
```

## CSV 檔案格式

- 常見資料儲存格式；每 **row 用換行**分隔、每 **column 用 `,`** 分隔。
- 標準寫法用內建 `csv` 模組（比手動 split 安全，能處理引號/逗號）：

```python
import csv
with open('data.csv', newline='', encoding='utf-8') as f:
    for row in csv.reader(f):      # 每 row 是 list
        print(row)

with open('out.csv', 'w', newline='', encoding='utf-8') as f:
    csv.writer(f).writerow(['name', 'score'])
```

## 讀寫檔案 open

| mode | 意義 |
|---|---|
| `'r'` | 讀（預設） |
| `'w'` | 寫（**覆蓋**） |
| `'a'` | 附加 |
| `'r+'` | 讀寫 |
| `'rb'/'wb'` | 二進位 |

```python
# 寫檔
with open('a.txt', 'w', encoding='utf-8') as f:
    f.write('hello\n')

# 讀檔（with 會自動關檔，最推薦）
with open('a.txt', 'r', encoding='utf-8') as f:
    content = f.read()          # 整份
    # f.readline() 一行 / f.readlines() 所有行成 list
    # for line in f: ...        逐行走訪
```

⚠️ 中文檔務必帶 `encoding='utf-8'`；用 `with` 確保關檔。延伸：os/shutil/json 檔案操作見 [cheat-內建模組](cheat-內建模組.md)。
