---
name: concept-資料探勘與kdd流程
description: M1 資料探勘簡介——DM 的定義與定位(AI/ML/統計/資料庫的交集)、KDD 五階段、ETL 三步、六大任務類型
metadata:
  type: concept
book: 成為AI科學家_資料探勘
chapter: M1 資料探勘簡介
---

# 資料探勘是什麼、KDD 與 ETL（M1）

> 定位：**資料探勘（Data Mining）本質上屬機器學習範疇**，是結合人工智慧、機器學習、統計學與資料庫的跨學科電腦科學分支，目的是**從大型資料集中發現模式**。

## 一句話主張

**把原始資料 → 資訊 → 洞察 → 知識。** 資料本身沒有價值，被轉成「可以做決定的東西」才有。

## KDD 流程五階段

KDD（Knowledge Discovery in Databases）＝資料探勘的上位流程，DM 只是其中**第四步**：

| # | 階段 | 在做什麼 |
|---|---|---|
| 1 | **選擇** Selection | 從資料源挑出目標資料集 |
| 2 | **預處理** Preprocessing | 清洗：遺失值、離群值、去重 |
| 3 | **變換** Transformation | 轉成演算法吃得下的格式（降維、編碼、衍生欄位） |
| 4 | **資料探勘** Data Mining | 套演算法找模式 |
| 5 | **解釋與評估** Interpretation/Evaluation | 轉成人看得懂的洞察 |

⭐ **第 2–3 階段（清洗＋變換）才是本課的主體**——M2–M4 四個模組全在講這兩步，M1 只是地圖。呼應 `principle-起点是需求不是数据` 與 `framework-预处理选型决策表`。

## ETL 三步（工程視角的同一件事）

| 步 | 名稱 | Python 工具 |
|---|---|---|
| **E** | Extract 抽取 | `pandas` 讀 CSV/Excel/JSON/XML/SQL |
| **T** | Transform 轉置 | `pandas` 篩選、合併、重塑、填補 |
| **L** | Load 載入 | `pandas` 寫回檔案／資料庫 |

> 本課的 ETL 工具**從頭到尾就是 Pandas 一套**。ETL 的 T 展開見 [framework-離群值與資料轉換](framework-離群值與資料轉換.md)。

## 課程流程（全課主軸）

```
資料輸入 → 清洗儲存 → 分析 → 圖表視覺化呈現
```

分析主題三類：**商業管理性分析**、**回歸預測**、**資料分類與分群**。

## 資料探勘任務類型

| 任務 | 代表方法 | vault 交叉 |
|---|---|---|
| **異常檢測** | 統計門檻、IsolationForest | `framework-數據異常檢測抓作弊`、[framework-離群值與資料轉換](framework-離群值與資料轉換.md) |
| **關聯規則學習** | Apriori | ⚠️ 頻繁≠有效，要看 Lift |
| **回歸** | 線性／邏輯回歸 | `concept-監督式演算法` |
| **分類** | 決策樹、貝葉斯、SVM | `framework-如何选分析算法` |
| **分群** | K-means、DBSCAN、階層式 | `concept-非監督式演算法` |

## 失敗模式

- ❌ **跳過 KDD 第 1–3 步直接套模型**——垃圾進垃圾出，模型再好也救不回來。
- ❌ **把 DM 當「跑演算法」**——第 5 步（解釋與評估）沒做，結果沒人用。
- ⚠️ 本課**不強調數學推導**，非核心模組的說明可能略過；要補理論深度得回 `MOC-掌握AI關鍵核心技術`。

## 相關

[MOC-資料探勘](MOC-資料探勘.md) · [cheat-pandas載入資料](cheat-pandas載入資料.md) · `framework-数据驱动运营大框架`
