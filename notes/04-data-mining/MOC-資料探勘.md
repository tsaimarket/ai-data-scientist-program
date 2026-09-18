---
name: MOC-資料探勘
description: 《成為 AI 科學家｜資料探勘速成攻略》12 模組導航根與萃取進度表（🎓 M1–M12 全數萃取完畢）
metadata:
  type: MOC
book: 成為AI科學家_資料探勘
---

# MOC — 資料探勘速成攻略

> 約 3h11m｜**143 頁 / 12 模組**
> 源＝`成為AI科學家_資料探勘速成攻略_課程講義_20210922.pdf`（有文字層）＋ `資料探勘補充包_教學小計畫.pdf`（34 頁，教學規劃與知識地圖）＋ Notion 課堂會議筆記＋ `DataMiningExercises/`（Tibame 範例 ipynb 與資料集）。
>
> **萃取策略＝上到哪萃到哪**（增量，不重抽）。2026-09-02 第一堂 **M1–M4**；2026-09-03 第二堂 **M5–M9**；**2026-09-18 第三堂 M10–M12 ＝ 🎓 全課完成**。
>
> ⚠️ 這門課屬既有 `AI資料科學家全方位學程` 線，與 [MOC-python語法速查](../01-python/MOC-python語法速查.md)、`MOC-網路爬蟲` 同系列的第三門線上自修課，**不是新開的第三條線**。

## 一句話定位

**爬蟲課教你「把資料抓下來」，這門課教你「把抓下來的東西變成能算的表」。** M1 是地圖（KDD／ETL），**M2–M4 是全課的重心＝資料清洗**，M5–M6 進到分析（EDA／關聯規則），M7–M11 補底層工具（NumPy／Matplotlib），M12 才把演算法一次帶過。

> ⭐ 全課的形狀＝**先讀懂流程（M1）→ 讀進來（M2）→ 洗乾淨（M3–M4）→ 看一眼（M5）→ 找關聯（M6）→ 補工具（M7–M11）→ 演算法簡介（M12）**。

## 進度表

| 模組 | 頁 | 主題 | 筆記 |
|---|---|---|---|
| **M1** | p6 | 資料探勘簡介（KDD 五階段、ETL、任務類型） | ✅ [concept-資料探勘與kdd流程](concept-資料探勘與kdd流程.md) |
| **M2** | p17 | 載入資料（CSV/Excel、JSON orient、XML、SQLite） | ✅ [cheat-pandas載入資料](cheat-pandas載入資料.md) |
| **M3** | p31 | 資料清洗與轉換（I）：篩選／文字／時間／遺失值 | ✅ [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md) |
| **M4** | p44 | 資料清洗與轉換（II）：離群值、轉換七類、合併重塑 | ✅ [framework-離群值與資料轉換](framework-離群值與資料轉換.md) |
| **M5** | p61 | 資料探索與視覺化（EDA） | ✅ [framework-eda探索流程](framework-eda探索流程.md) |
| **M6** | p69 | 關聯分析（Market Basket） | ✅ [framework-關聯分析與購物籃](framework-關聯分析與購物籃.md) |
| **M7** | p79 | NumPy 套件（ndarray、broadcasting） | ✅ [cheat-numpy-ndarray](cheat-numpy-ndarray.md) |
| **M8** | p89 | ndarray 操作（index／mask／axis／統計） | ✅ 同上 |
| **M9** | p101 | ndarray 合併與轉換（concatenate／stack／astype） | ✅ 同上 |
| **M10** | p109 | Pandas 物件運算（axis in DataFrame） | ✅ [cheat-pandas物件運算與型別](cheat-pandas物件運算與型別.md) |
| **M11** | p121 | Matplotlib 套件（figure） | ✅ [cheat-matplotlib繪圖](cheat-matplotlib繪圖.md) |
| **M12** | p130 | 更多資料探勘演算法簡介（線性／羅吉斯迴歸…） | ✅ [concept-演算法簡介與sklearn對照](concept-演算法簡介與sklearn對照.md) |

課程總覽與環境／套件地圖：[reference-課程環境與工具](reference-課程環境與工具.md)

## 清洗決策骨架（M1–M4 的收束）

拿到一份新資料時的判斷順序：

```
1. 讀得進來嗎？      → CSV/Excel 直接讀；JSON 先確認 orient；XML 要自己解；DB 走 SQLAlchemy（M2）
2. 先跑 describe()   → 看型別、範圍、缺幾筆（M2）
3. 有洞嗎？          → 類別欄 fillna／偽遺失值 replace／連續值 interpolate（M3）
4. 有離群值嗎？      → IQR×1.5 或 ±3σ 或 Z-score 抓出來
                       ⚠️ 抓到不等於能刪 → 先問「為什麼會有這個點」（M4）
5. 形狀對嗎？        → 去重／格式／衍生／合併 merge-join-concat／重塑 stack（M4）
6. 資料大過 RAM 了嗎？→ 是 ⇒ 該離開 Pandas（M4 的天花板）
```

## 分析骨架（M5–M9 的收束）

洗完之後接著做什麼：

```
7. 先看一眼          → 目標分布 + 各欄直方 + 關聯矩陣 + 類別欄 box plot（M5）
8. 砍特徵            → 缺失 >30% 刪／值全擠在一起刪／兩欄互相高相關留一個（M5）
9. 找共現規則        → Support 篩掉罕見、Confidence 看準不準
                       ⭐ 只有 Lift > 1 才代表「有加強效果」（M6）
10. 算不動時          → 回到 NumPy：向量化 / broadcasting / mask，別寫 for（M7–M9）
                       ⚠️ axis=0 消列留欄、axis=1 消欄留列——記錯不會報錯，只會全錯
```

⭐ **兩條「有數字但方向全錯」的暗坑**：M6 的 Lift≈1（熱門商品跟誰都同時出現）、M8 的 axis 記反。兩者都不報錯。

## 工具與演算法骨架（M10–M12 的收束）

分析完之後、要開始算與畫：

```
11. 換到 DataFrame 算   → axis 口訣同 ndarray：axis ＝ 被消滅的那個軸
                          axis=0 別名有兩個(index/rows)、axis=1 只有 columns（M10）
12. 型別對不對？        → object 欄＝混進了非數值；讀檔當下用 dtype= / converters= 轉掉
                          ⚠️ errors='coerce' 會把轉不動的靜默變成 NaN，轉完要數一次（M10）
13. 去重                → duplicated() 回布林 Series，用 ~ 取反保留
                          ⚠️ NaN 會被當成重複 → 先補值再去重，否則誤刪（M10）
14. 畫出來              → figure → subplot(列,行,序號，⚠️從 1 起算) → plot('ob--') → label/legend（M11）
                          ⚠️ 選哪張圖不在本課，看 framework-基本統計圖選型
15. 上演算法            → 監督式看依變數型別：連續→線性迴歸、類別→羅吉斯/決策樹/NB/SVM
                          非監督式：想切 N 群→K-means；要抓異常→DBSCAN（M12）
                          ⭐ SVM 一定要 make_pipeline(StandardScaler(), SVC())
```

⭐ **三條「有數字但方向全錯且不報錯」的暗坑（全課總表）**：

| # | 坑 | 錯的長相 |
|---|---|---|
| 1 | **M6 的 Lift≈1** | Support／Confidence 很漂亮，但熱門商品跟誰都同時出現＝零資訊 |
| 2 | **M8／M10 的 axis 記反** | 加總方向整個相反，數字照樣算得出來 |
| 3 | 🔴 **M12 的 DBSCAN 雜訊標記** | 是 **`-1`** 不是 0；照「雜訊＝0」過濾會**刪掉整個第一群、把雜訊全留下** |

## 跨書交叉

| 這門課 | vault 已有的深化 |
|---|---|
| KDD／ETL 流程 | `framework-数据驱动运营大框架`、`principle-起点是需求不是数据` |
| 預處理選型 | ⭐ `framework-预处理选型决策表`（標準化／不均衡／降維／離散化「情境→選哪個」總表） |
| 離群值≠雜訊 | `framework-數據異常檢測抓作弊`、`principle-稀有事件是詐欺之母` |
| 時間欄與 resample | ⚠️ `principle-时间序列不适合复杂商业环境`（＝A/B 必須並行的理論依據） |
| M5 EDA 選圖 | `framework-基本統計圖選型`（條形/餅/折線三大誤用）、`framework-資料視覺化選圖` |
| M6 關聯分析 | ⚠️ 頻繁規則≠有效，要看 Lift（`framework-如何选分析算法`）；商業用法見 `framework-關聯選品與波士頓矩陣`、經典案例 `case-啤酒尿布` |
| M7–M9 NumPy | [MOC-python語法速查](../01-python/MOC-python語法速查.md)（Python 底座）；矩陣/相關係數是投資組合分散與特徵共線性分析的基礎 |
| M12 演算法簡介 | ⭐ **深度一律回 `MOC-tibame訓練營`**（同一批演算法的完整版）；概念地圖 `concept-監督式演算法`、`concept-非監督式演算法`；選型 `framework-如何选分析算法` |
| pandas 實作 | `cheat-pandas-dataframe與series`、`cheat-pandas過濾器與csv`（爬蟲課 M16–M17，較淺） |

> 定位差異：**這門課是「pandas 清洗的系統版」**——爬蟲課的 pandas 只是為了存 CSV，《Python数据分析与数据化运营》是「選哪個方法」的決策層，這門課補的是中間那層**逐個 API 的操作手感**。

## 對接

- **時間序列資料**：M3 的 `resample()` 與時間 index 是 K 線與賽程資料的基本功；⚠️ 但 M3 的時間對比要配 `principle-时间序列不适合复杂商业环境` 一起讀。
- **政府採購標案雷達**：爬回來的標案表就是靠這套清洗流程去重與打分。
