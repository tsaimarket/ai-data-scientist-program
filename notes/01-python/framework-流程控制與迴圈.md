---
name: framework-流程控制與迴圈
description: if/elif/else 條件、while/for/range 迴圈、break/continue、巢狀結構速查
metadata:
  type: framework
book: 快速闖關Python語法世界
chapter: M8-9
---

# 流程控制與迴圈速查

控制流程＝決定哪些區塊在哪些條件下被執行。靠**縮排**界定區塊（4 空白）。

## 條件判斷 if / elif / else

```python
grade = 80
if grade >= 70:
    print('pass!')
elif grade < 60:
    print('fail...')
else:
    print('普通')
```
- `if` 後接 True/False 條件 + `:`，縮排內為條件成立時執行的區塊。
- `elif`：前面條件不成立時再檢查；`else`：全部不成立時執行。

### 巢狀

```python
if cond1:
    if condA:
        ...
    elif condB:
        ...
    else:
        ...
else:
    ...
```

## while 迴圈

條件為真時重複執行：
```python
i = 0
while i < 5:
    print(i)
    i += 1
```

## for 迴圈（迭代序列）

```python
for x in [1, 2, 3]:        # 可迭代 list/tuple/dict/str...
    print(x)
```

### range（常配 for）

```python
range(n)              # 0..n-1
range(start, end, step)
for i in range(1, 10, 2):  # 1,3,5,7,9
    print(i)
```

## break / continue

| 關鍵字 | 作用 |
|---|---|
| `break` | 立即**中斷整個迴圈** |
| `continue` | **跳過本次剩餘**、直接進下一輪 |

```python
for i in range(10):
    if i == 5: break       # 到 5 就停
    if i % 2: continue     # 奇數跳過
    print(i)               # 0 2 4
```

## 巢狀迴圈

迴圈裡有迴圈；內層可搭 `continue`/`break`/`if-else`。常用於二維資料、九九乘法表。

延伸：條件用的布林/比較運算見 [framework-運算元與優先序](framework-運算元與優先序.md)；走訪容器見 [cheat-list-tuple-set-dict](cheat-list-tuple-set-dict.md)。
