---
name: concept-執行緒與平行處理
description: 執行緒/子執行緒概念、平行處理加速、threading 與 multiprocessing 實作速查
metadata:
  type: concept
book: 快速闖關Python語法世界
chapter: M24
---

# 執行緒與平行處理

## 概念

- **執行緒 thread**：正在執行的程式流。**子執行緒**用來在程式裡同時跑其他工作。
- **平行處理**：把工作分給多個 CPU/核心同時算，加快速度。

```
       ┌── Task 1 → CPU ─┐
任務分配 ├── Task 2 → CPU ─┤→ 合併 result
       └── Task N → CPU ─┘
```

## subprocess — 叫起「外部程式」當子行程

跟 threading 不同層：threading 是在**同一個 Python 程式內**開流程，subprocess 是**啟動另一支執行檔**。

```python
import subprocess
subprocess.Popen('notepad.exe')          # 非阻塞：叫起記事本後程式繼續跑
subprocess.run(['ls', '-l'], check=True) # 阻塞：等它跑完再往下
```

| 需求 | 用 |
|---|---|
| 叫起就好、不等它 | `Popen()` |
| 要拿結果/確認成功 | `run(..., capture_output=True, check=True)` |

## threading（多執行緒）

```python
import threading

def work(n):
    print(f'task {n}')

threads = [threading.Thread(target=work, args=(i,)) for i in range(3)]
for t in threads: t.start()
for t in threads: t.join()    # 等全部跑完
```

## multiprocessing（多行程，真平行）

```python
from multiprocessing import Pool

def square(x): return x * x

with Pool(4) as p:
    print(p.map(square, [1,2,3,4]))   # [1,4,9,16]
```

## ⚠️ GIL 重點（課程未強調，但必知）

| 工作型態 | 該用 | 原因 |
|---|---|---|
| I/O 密集（網路/讀檔/爬蟲） | `threading` / asyncio | 等待時可切換，執行緒夠用 |
| CPU 密集（大量運算/OCR） | `multiprocessing` | Python 有 **GIL**，多執行緒無法真正並行算 CPU；要多行程才吃滿多核 |

→ 對接 vault 經驗：OCR 掃描書（CPU 密集）就是用 `multiprocessing.Pool` + 每 worker 限 1 執行緒，正是這條規則。模組安裝見 [cheat-第三方與自定義模組](cheat-第三方與自定義模組.md)。
