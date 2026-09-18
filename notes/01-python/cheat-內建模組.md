---
name: cheat-內建模組
description: datetime/math/random、os/shutil/json、time/sys/zipfile、logging 級別與格式 速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M17-20
---

# 內建模組速查（datetime/math/random/os/shutil/json/time/sys/zipfile/logging）

## datetime（時間）

```python
from datetime import datetime
now = datetime.now()
now.strftime('%Y-%m-%d %H:%M:%S')   # 格式化輸出
datetime.strptime('2018-12-31', '%Y-%m-%d')  # 字串轉時間
```

| 格式碼 | 意義 | 例 |
|---|---|---|
| `%Y`/`%y` | 年(4位/2位) | 2018/18 |
| `%m`/`%B`/`%b` | 月(數字/全名/縮寫) | 12/December/Dec |
| `%d` | 日 01-31 | 31 |
| `%H`/`%I` | 時(24/12) | 17/05 |
| `%w`/`%A`/`%a` | 星期(數字/全名/縮寫) | 3/Wednesday/Wed |

## math（數學）

```python
import math
math.sqrt(16)  math.pi  math.ceil(2.1)  math.floor(2.9)
math.sin(x)  math.cos(x)  math.log(x)  math.pow(2,3)
```

## random（亂數）

```python
import random
random.random()          # [0,1) 浮點
random.randint(1, 6)     # 含兩端整數
random.choice(lst)       # 隨機選一
random.shuffle(lst)      # 原地洗牌
random.seed(42)          # 固定種子 → 每次結果一樣（可重現）
```

## os（作業系統）

| 方法 | 作用 |
|---|---|
| `os.getcwd()` | 目前工作目錄 |
| `os.listdir(path)` | 列目錄內容 |
| `os.mkdir(path)` / `os.makedirs(path)` | 建目錄 / 遞迴建 |
| `os.system(cmd)` | 執行 shell 指令 |
| `os.environ` | 環境變數 |
| `os.path.join(a,b)` | 組路徑（跨平台） |

## shutil（檔案高階操作）

| 方法 | 作用 |
|---|---|
| `shutil.copy(src,dst)` | 複製檔案 |
| `shutil.copytree(src,dst)` | 複製整個目錄 |
| `shutil.move(src,dst)` | 移動 |
| `shutil.rmtree(path)` | 刪整個目錄 |

## json（資料交換格式）

```python
import json
json.dumps(obj)    # 物件 → JSON 字串
json.loads(s)      # JSON 字串 → 物件
json.dump(obj, f)  # 寫檔
json.load(f)       # 讀檔
```

## time / sys / zipfile

```python
import time
time.time()        # 自 UTC 紀元起的秒數
time.sleep(1)      # 暫停 1 秒

import sys
sys.argv           # 命令列參數 list
sys.path           # 模組搜尋路徑
sys.exit()         # 結束程式

import zipfile
with zipfile.ZipFile('a.zip', 'w') as z:
    z.write('file.txt')        # 壓縮
with zipfile.ZipFile('a.zip') as z:
    z.extractall('out/')       # 解壓
```

## logging（日誌，取代滿天 print）

部署時不用一個個刪 print，用 logging 統一管控。

| 級別 | 數值 | 函數 |
|---|---|---|
| NOTSET | 0 | — |
| DEBUG | 10 | `logging.debug()` |
| INFO | 20 | `logging.info()` |
| WARNING | 30 | `logging.warning()`（預設門檻） |
| ERROR | 40 | `logging.error()` |
| CRITICAL | 50 | `logging.critical()` |

```python
import logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s %(levelname)s %(message)s')
logging.info('啟動')
```

常見格式碼：`%(asctime)s` 時間、`%(levelname)s` 級別、`%(message)s` 訊息、`%(filename)s`/`%(funcName)s`/`%(lineno)d` 檔名/函數/行號。

延伸：第三方套件安裝/jieba 見 [cheat-第三方與自定義模組](cheat-第三方與自定義模組.md)；多核加速見 [concept-執行緒與平行處理](concept-執行緒與平行處理.md)。
