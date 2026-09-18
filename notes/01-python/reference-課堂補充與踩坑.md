---
name: reference-課堂補充與踩坑
description: 《快速闖關 Python 語法世界》課堂口述才有、講義沒寫的補充與踩坑總表（含 Anaconda/Colab 課前課）
metadata:
  type: reference
book: 快速闖關Python語法世界
chapter: 課前課+M1-M30
---

# 課堂補充與踩坑

講義 PDF 已萃取成 18 篇速查筆記；**這篇只收「講義沒寫、講師口頭講的」**——環境課細節、踩坑、老舊寫法的現代替代。

## 環境（課前課，講義沒有這堂）

| 主題 | 重點 |
|---|---|
| 課程定位 | 兩小時課前課，節奏刻意放慢確保每人裝起來；支援 Windows 與 Mac |
| Anaconda 安裝 | 全程預設「下一步」；初學者建議勾選 **Add Anaconda to PATH**；裝完開 Anaconda Navigator 確認 |
| 虛擬環境 | Environment → Create → 命名（課堂用 `PythonCosmos`）。**`base` 是本機環境，不建議直接用** |
| 套件安裝的**頭號坑** | 一定要在**虛擬環境的 Terminal**（環境旁箭頭 → Open Terminal）跑 `pip install`。判斷方法：**前綴會顯示環境名稱**；在系統 cmd 裝會裝到別的地方 |
| 裝完套件沒生效 | Kernel → Restart |
| 檔案放哪 | Jupyter 預設**不好切到 D 槽** → Anaconda 與課程檔案都放 **C 槽**，做完再複製/剪下搬走 |
| 語系 | 日文語系電腦可用，但要確認能正確顯示課程中的中文註解 |
| Colab 備援 | 本機裝不起來就用；「檔案 → 上傳筆記本」可丟本機 `.ipynb` 上去 |
| ⚠️ Colab 最大風險 | 程式碼在 Google 雲端，**長時間閒置會斷線、可能掉資料** → 養成「檔案 → 下載 → 下載 .ipynb」的習慣 |

## Jupyter 執行狀態判讀

| 顯示 | 意義 |
|---|---|
| `In[3]` | 已執行成功（數字＝執行順序） |
| `In[ ]` | 尚未執行 |
| `In[*]` | 執行中；**若一直卡星號 → 多半進了無窮迴圈 → Kernel → Restart** |

## 語法層的口述補充

| 主題 | 講義沒說的部分 |
|---|---|
| `isidentifier()` | **有缺陷**：`"for".isidentifier()` 回 `True`。合法性檢查要再加 `not keyword.iskeyword(name)` |
| `.append()` vs `.extend()` | `a.append(b)` 把整個 list `b` 當**一個元素**塞進去（變巢狀）；合併請用 `.extend()` |
| `set.pop()` | 因為 set 無序，pop 是**隨機**移除一個，不是移除最後一個 |
| 非 0 數字 | 在條件判斷中視為 `True` |
| 多個獨立 `if` vs `if/elif/else` | 前者各自判斷（可能多個都跑或都不跑）；後者**強迫多選一** |
| 巢狀迴圈印圖形 | 換行的 `print()` 必須放在**外層**迴圈結尾 |
| 遞迴上限 | Python 預設 **3000 次**，超過 `RecursionError`；`sys.setrecursionlimit()` 可調（但通常代表該改寫成迴圈/DP） |
| `if __name__ == '__main__': main()` | 判斷是否為「直接執行」而非被 import 的主程式慣例 |
| 多重繼承 | 多個父類別有同名 method 時，以**第一個繼承的類別**為優先 |
| `super().__init__()` | 子類別自己寫 `__init__` 後，父類別的初始化會被蓋掉，要用 super 叫回來 |
| lambda 的意義 | 不只是省字——`map`/`reduce` 是**大數據分析與分散式運算**的基礎思路 |
| `random.seed(n)` | 固定種子讓每次結果一致，**方便 debug** |
| logging 的意義 | 取代開發時滿地的 `print`；分等級後部署時一行就關掉不重要輸出 |
| 學習態度 | list 加總、找最大值這類題，講師要求**先手寫原理**再用 `sum()`/`max()` |

## 安裝與口誤（2026-07-31 兩堂補登）

| 主題 | 重點 |
|---|---|
| pip 移除套件 | 課堂口述的 `pip install <pkg> --uninstall` 是**錯的**，正確是 `pip uninstall <pkg>` |
| SQLite Browser | 抓 **NoInstaller 免安裝版**（依 OS 與位元選），解壓後直接跑 `db-browser-for-sqlite.exe`；用「打開資料庫 → Browse Data」看表 |
| MySQL 官方安裝 | Windows 版**要 Oracle 帳號**才能下載 → 本機已有 Docker `mysql`，別再手裝 |
| MySQL 介接 API | 課堂用 `pip install mysqlclient`（即 MySQLdb），Py3 常編譯失敗 → 用 `pymysql` |
| PyCharm | 抓 **Community 免費版**；首次建專案跳 Permission Denial 按 OK 忽略即可 |

## 老舊寫法 → 現代替代

| 講義 | 問題 | 替代 |
|---|---|---|
| `pytube` | YouTube 一改版就壞 | `yt-dlp` |
| `MySQLdb` / `mysqlclient` | Python 3 下常裝不起來 | `pymysql`、`mysql-connector-python`（API 幾乎相同） |
| `"{}".format()` / `%` | 仍可用 | 日常優先 **f-string** |
| Python 3.6 / 3.7 | 已 EOL | 本訓練營用 `ds`（Python 3.13） |
| Tesseract 需另裝執行檔 | 兩步驟才裝得起來,且 **pip 裝完還要在程式裡設 `tesseract_cmd` 指到 exe** 才會動 | 中文場景可考慮 `rapidocr-onnxruntime`（自帶模型，免裝引擎） |
| 手動裝 MySQL（需 Oracle 帳號） | 麻煩 | 本機已有 Docker `mysql`（`127.0.0.1:3306`, root/<你的密碼>, adminer 在 :8080） |

## 課程進度 vs Notion 課堂筆記對照

| Notion 頁 | 涵蓋 | 日期 |
|---|---|---|
| 開發環境建置 | 課前課（Anaconda / Jupyter / Colab） | — |
| 模組 1~3 | 語言概念、環境、基礎語法 | — |
| 模組 4~7 | 運算元、資料型別 | — |
| 模組 8~9 | 流程控制、迴圈 | — |
| 模組 10~13 | 字串格式、regex、函數、lambda、遞迴 | — |
| 模組 14~15 | 物件導向 | — |
| 模組 16~20 | 例外、檔案、內建模組、logging | 2026-06-30 |
| [Python 程式語言:模組21~24](https://app.notion.com/p/3aeadb8aefde8052902ced096ea17bc6) | jieba/PIL/pytube/qrcode/OCR、自訂模組、執行緒 ≈ M21–24 | 2026-07-31 16:54 |
| [Python 程式語言:模組25~30](https://app.notion.com/p/3aeadb8aefde80db9c96ee0b2d3b368c) | SQLite、MySQL、Tkinter、PyCharm/Replit ≈ M25–30 | 2026-07-31 17:12 |

> 最後一堂錄音中段混入與課程無關的旅遊對話，Notion 摘要已註明略去。

---

延伸：語法查表見 [MOC-python語法速查](MOC-python語法速查.md)；習題解法見 [drill-習題解法彙編](drill-習題解法彙編.md)；線性教學版見同層 `Python完整教學_M1-M30.md`。
