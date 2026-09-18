---
name: concept-遞迴與動態規劃
description: 遞迴函式概念與實作、重複子問題、動態規劃以記憶體換時間
metadata:
  type: concept
book: 快速闖關Python語法世界
chapter: M13
---

# 遞迴與動態規劃

## 遞迴 recursion

遞迴函式＝**呼叫自己**的函式。需有：① 基底條件（停止點）② 往基底逼近的遞迴呼叫。

```python
def factorial(n):
    if n == 1:          # 基底條件，沒有會無限遞迴
        return 1
    return n * factorial(n - 1)
```

展開 `factorial(5)`：
```
5 * factorial(4)
5 * (4 * factorial(3))
... → 5 * (4 * (3 * (2 * 1))) → 120
```

## 重複子問題的浪費

純遞迴費氏數列會**重複計算**同一子問題（指數時間）：
```python
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)   # fib(n-2) 被算很多次
```

## 動態規劃 DP

把每一步子問題結果**存進記憶體**，下次直接查表，不重算 → 以空間換時間。

```python
# 記憶化（top-down）
memo = {}
def fib(n):
    if n < 2: return n
    if n in memo: return memo[n]
    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]

# 表格法（bottom-up）
def fib(n):
    dp = [0, 1]
    for i in range(2, n+1):
        dp.append(dp[i-1] + dp[i-2])
    return dp[n]
```

核心觀念：**重疊子問題 + 最優子結構** → 用 DP。延伸：遞迴是函數呼叫自己，函數基礎見 [framework-函數](framework-函數.md)。
