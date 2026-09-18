---
name: MOC-python語法速查
description: 《快速闖關 Python 語法世界》30 模組語法速查總導航；按主題分群連到各速查筆記
metadata:
  type: MOC
book: 快速闖關Python語法世界
---

# MOC｜Python 語法世界速查

《快速闖關 Python 語法世界，程式實作不頭痛》闖關課程講義（30 模組，324 頁）的 form-A 速查萃取。**定位＝語法查表手冊**：基礎部分（變數/print/if）刻意壓縮，火力集中在進階廣度主題（regex、OOP、模組、SQL、執行緒）。使用者已會基礎 Python，這裡是「忘了某個語法回來查」用。

## 模組 → 筆記對照

| 模組 | 主題 | 速查筆記 |
|---|---|---|
| M1–2、M30 | 程式語言概念 / 開發環境 | [concept-python執行與環境](concept-python執行與環境.md) |
| M3 | keyword / identifier / literal / 註解 / 縮排 / print·input | [cheat-基礎語法](cheat-基礎語法.md) |
| M4 | 七類運算元 + 優先序 ⭐ | [framework-運算元與優先序](framework-運算元與優先序.md) |
| M5、M7 | 數值/布林型別、mutable、型別轉換 | [cheat-資料型別總覽](cheat-資料型別總覽.md) |
| M6、M10 | 字串索引/切片/函數 + 字串格式化 | [cheat-字串](cheat-字串.md) |
| M6–7 | list / tuple / set / dict 操作 | [cheat-list-tuple-set-dict](cheat-list-tuple-set-dict.md) |
| M8–9 | if/elif/else、while/for/range、break/continue、巢狀 | [framework-流程控制與迴圈](framework-流程控制與迴圈.md) |
| M10 | 正規表達式 re ⭐ | [framework-正規表達式](framework-正規表達式.md) |
| M11–12 | 自訂/內建函數、*args/**kwargs、全域區域、lambda+map/filter/reduce ⭐ | [framework-函數](framework-函數.md) |
| M13 | 遞迴、動態規劃 | [concept-遞迴與動態規劃](concept-遞迴與動態規劃.md) |
| M14–15 | class、封裝/繼承/多型 ⭐ | [framework-物件導向](framework-物件導向.md) |
| M16 | try/except/raise/assert、CSV、檔案讀寫 | [cheat-例外處理與檔案io](cheat-例外處理與檔案io.md) |
| M17–20 | datetime/math/random、os/shutil/json、time/sys/zipfile、logging ⭐ | [cheat-內建模組](cheat-內建模組.md) |
| M21–23 | pip、jieba/Pillow/pytube/qrcode/tesseract、import/from import | [cheat-第三方與自定義模組](cheat-第三方與自定義模組.md) |
| M24 | 執行緒、平行處理 | [concept-執行緒與平行處理](concept-執行緒與平行處理.md) |
| M25–27 | SQLite、SQL CRUD、MySQL（sqlite3 / pymysql） ⭐ | [framework-sql資料庫](framework-sql資料庫.md) |
| M28–29 | Tkinter GUI 元件 | [cheat-tkinter-gui](cheat-tkinter-gui.md) |

## 課堂層增量（源＝Notion 課堂會議筆記 9 篇）

講義只給題目、環境細節與踩坑只在課堂口述——這兩篇補的是講義沒有的那層：

| 筆記 | 內容 |
|---|---|
| [drill-習題解法彙編](drill-習題解法彙編.md) | 全課 Practice 題目的**解題思路**（M3–M16，約 40 題） |
| [reference-課堂補充與踩坑](reference-課堂補充與踩坑.md) | 課前環境課（Anaconda/Colab）、Jupyter 狀態判讀、口述補充、老舊寫法的現代替代 |

## 想「從頭讀一遍」而不是查表

→ 同層 **`Python完整教學_M1-M30.md`**：線性教學版（概念→語法→可跑範例→坑），八個 Part 走完 30 模組，附全課踩坑總表與版本老舊處對照。本 MOC 底下的筆記是查表用，那份是通讀用。

## 跨資料夾關聯

- **這是 TibaMe《18週成為AI資料科學家》訓練營的 L1 Python 底座**——排程文件已於 2026-09-01 刪除；動手環境見 [`docs/environment.md`](../../docs/environment.md)。
- M25–27 的 SQL/MySQL 對接訓練營 L2 MySQL；本機已有 Docker `mysql`（root/<你的密碼>, 127.0.0.1:3306），可直接拿 [framework-sql資料庫](framework-sql資料庫.md) 的 CRUD 練。
- 想把語法用到資料分析落地 → 看 `Data Science/Python数据分析与数据化运营/`（同個技能樹的應用層 playbook）。

## ⚠️ 版本提醒

講義部分頁面停在舊版寫法：`pytube`（YouTube 改版後常壞，現多用 yt-dlp）、`MySQLdb`/`MySQLdb` 套件（Python3 改用 `pymysql` 或 `mysql-connector-python`）、`tesseract` 需另裝引擎。各筆記已標註現代等價寫法。
