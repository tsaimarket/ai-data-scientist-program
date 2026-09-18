---
name: reference-課程環境與工具
description: 課前準備與工具地圖——Colab/Anaconda/Kaggle 三種環境、NumPy/Pandas/Matplotlib/Scikit-Learn 四套件分工、12 模組大綱、補充包的資料科學知識地圖
metadata:
  type: reference
book: 成為AI科學家_資料探勘
chapter: 課程規劃與範疇
---

# 課程環境、工具與知識地圖

> 課程：**《成為 AI 科學家｜資料探勘速成攻略，輕鬆駕馭資料分析與實務應用》**
> 約 3h11m｜143 頁 **12 模組**｜屬 `AI資料科學家全方位學程` 線上自修課第三門。

## 課前準備（Checklist）

- [ ] **Google 帳號 + Google Drive** — 主要學習環境是 **Google Colab**
- [ ] （可選）**Anaconda**（含 Python 3 與 Jupyter）— 想在本機跑就裝；⚠️ 講義提醒要考量自身電腦是否有足夠 GPU/TPU
- [ ] （建議）**Kaggle 帳號** — 部分課程範例放在 kaggle.com 上

> 💡 本機若已備妥 conda 環境與 VSCode notebook（設定見 [`docs/environment.md`](../../docs/environment.md)），**不必真的用 Colab**；範例檔已在同資料夾 `DataMiningExercises/`（`Tibame_Pandas.ipynb`、`PandasSQL/XML`、`MarketBasketAnalysis.ipynb`、`melb_data.csv`、`train.csv` 等）。

## 預備知識門檻

| 需要 | 程度 |
|---|---|
| **Python 程式語言** | 變數／流程／函式／套件；已備＝[MOC-python語法速查](../01-python/MOC-python語法速查.md) |
| **機率與統計** | 平均、標準差、分位數、常態分佈 |
| **數學** | 基本幾何、線性代數、一點點微積分 |

⚠️ **本課不強調數學推導**，非核心模組可能略過；要補理論深度回 `MOC-掌握AI關鍵核心技術`。
📈 **進階方向**（講師標示的承先啟後）：機器學習 → 深度學習。

## 12 模組大綱

| | | |
|---|---|---|
| 1. 資料探勘簡介 | 5. 資料探索與視覺化 | 9. ndarray 合併與轉換 |
| 2. 載入資料 | 6. 關聯分析 | 10. Pandas 物件運算 |
| 3. 資料清洗與轉換（I） | 7. NumPy 套件 | 11. Matplotlib 套件 |
| 4. 資料清洗與轉換（II） | 8. ndarray 操作 | 12. 更多資料探勘演算法簡介 |

頁碼與萃取進度見 [MOC-資料探勘](MOC-資料探勘.md)。

## 四大套件地圖

| 套件 | 定位 | 在本課的角色 |
|---|---|---|
| **Pandas** | 資料讀取、轉換、載入（**ETL**） | ⭐ **主角**，M2–M4、M10；兩種結構＝**Series（一維）／DataFrame（二維）** |
| **NumPy** | 科學運算基礎 | 高效能多維陣列、線性代數、傅立葉轉換；M7–M9；⭐ 其他三個都站在它上面 |
| **Matplotlib / PyPlot** | Python 繪圖套件 | M11；「MATLAB 的 Python 版」 |
| **Scikit-Learn** | 開源機器學習工具包 | M12；基於 NumPy、SciPy、Matplotlib |

```
NumPy（陣列底座）
  ├─ Pandas（表格 ETL）──→ Matplotlib（畫圖）
  └─ SciPy ──→ Scikit-Learn（建模）
```

## 一句話定位（講師原話改寫）

> **資料探勘＝知識的煉油工業**——從資料**提煉**（Pandas）、**分析**（NumPy／SKLearn）、再**展現在報表圖表**（Matplotlib）上。
> 在機器學習盛行之前，Data Mining 就是探索資料洞見（Insight）與知識（Knowledge）最重要的課程與工具。

## 資料科學知識示意地圖（補充包 p4）

出自 `資料探勘補充包_教學小計畫.pdf`（34 頁，同講師的教學規劃版，另含「在 TensorFlow 上進行機器學習」）：

```
先備知識：數學基礎（線代／機率統計／微積分）· Python · 資料庫/網路/IT架構
   ↓
輸入 ETL(Pandas) → 處理(NumPy) → 圖表(Matplotlib)
   ↓
資料探勘：EDA · 特徵擷取 · 關聯規則分析 · A/B 測試
   ↓
機器學習演算法(Scikit-Learn)：迴歸/預測 · 分類 · 分群 · 組合(Ensemble) · 感知器/神經網路
   ↓
深度學習：激勵函數 · 損失函數 · 優化 · DNN/CNN/RNN · NLP · GAN/VAE · 強化學習
```

⭐ **這張圖說明本課在整個學程的座標**：它是「ETL → 資料探勘」那兩層，往下接 `MOC-網路爬蟲`（資料從哪來），往上接 `MOC-掌握AI關鍵核心技術`（ML/DL）。

## 全課流程

```
資料輸入 → 清洗儲存 → 分析 → 圖表視覺化呈現
```

分析主題三類：**商業管理性分析（關聯分析）**、**回歸預測**、**資料分類與分群**。

## 相關

[MOC-資料探勘](MOC-資料探勘.md) · [concept-資料探勘與kdd流程](concept-資料探勘與kdd流程.md) · `MOC-網路爬蟲` · [MOC-python語法速查](../01-python/MOC-python語法速查.md)
