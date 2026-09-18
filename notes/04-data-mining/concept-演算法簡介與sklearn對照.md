---
name: concept-演算法簡介與sklearn對照
description: M12 演算法簡介——線性/羅吉斯迴歸、決策樹/樸素貝葉斯/SVM、階層式分群/K-means/DBSCAN 的 sklearn 最小可跑範例與參數對照。⚠️ 純簡介層，深度一律回 TibaMe 訓練營筆記；含一處課堂口述與講義不符的更正（DBSCAN 雜訊標記是 -1 不是 0）
metadata:
  type: concept
book: 成為AI科學家_資料探勘
chapter: M12 更多資料探勘演算法簡介（p130–143）
---

# 演算法簡介與 sklearn 對照（M12）

> ⚠️ **定位先講清楚：這是「簡介」不是「教學」。** 全模組 14 頁走完 8 個演算法，每個演算法只有三到五行 sklearn 範例，**沒有數學、沒有調參、沒有評估指標**。
> 🔴 **深度一律去 `MOC-tibame訓練營`**——同一批演算法在 TibaMe L4／L5 有完整版（含 cost function、混淆矩陣、正則化、集成）。本則只保留**這門課獨有的兩樣東西：最小可跑骨架，與講師的兩句口述判斷。**

## 一、監督式：迴歸兩兄弟

| | 線性迴歸 Linear Regression | 羅吉斯迴歸 Logistic Regression |
|---|---|---|
| 依變數 | **連續型**（房價） | **類別型**（腫瘤良／惡性） |
| 解決 | 迴歸（預測）問題 | 分類問題 |
| 輸出 | 數值 | ⭐ **0～1 之間的機率** |

```python
from sklearn.linear_model import LinearRegression
reg = LinearRegression().fit(X, y)
reg.score(X, y)      # 1.0（此為人造資料）
reg.coef_            # 斜率 array([1., 2.])
reg.intercept_       # 截距 3.0
reg.predict([[3,5]]) # ⭐ 外插預測 array([16.])
```

```python
from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression
X, y = load_iris(return_X_y=True)     # 150 筆、4 特徵、3 類別
clf = LogisticRegression(random_state=0).fit(X, y)
clf.predict(X[:2, :])        # array([0, 0])
clf.predict_proba(X[:2, :])  # 每一類的機率
clf.score(X, y)              # 0.9733
```

> ⭐ **講師的兩句口述（講義上沒有，是這堂課的增量）**：
> 1. **「真實世界的 score 不會達到 1.0」**——講義例題之所以是 1.0，是因為 `y` 本來就是用 `1*x0 + 2*x1 + 3` 造出來的。看到 1.0 要先懷疑資料洩漏，不是高興。
> 2. **「模型的核心價值在於外插預測」**——`score` 只是回頭看擬合得好不好，`predict()` 才是它存在的理由。

## 二、監督式：分類三種

講義定義（原文口徑）：

| 演算法 | 一句話 | sklearn |
|---|---|---|
| **決策樹** | 定下一個最初的質點，**根據最大亂度下降分叉**，循環反覆 | `DecisionTreeClassifier` |
| **樸素貝葉斯** | 要推「B 發生下 A 的機率」，可透過「A 發生下 B 的機率」再用**貝葉斯定理**換算 | `GaussianNB` |
| **SVM** | 調整**核函數**參數，把二維線性不可分的問題轉成**多維線性可分**，再找最優切割平面 | `SVC` |

```python
# 決策樹 ＋ 10 折交叉驗證
from sklearn.model_selection import cross_val_score
from sklearn.tree import DecisionTreeClassifier
clf = DecisionTreeClassifier(random_state=0)
cross_val_score(clf, iris.data, iris.target, cv=10)
# array([1.0, 0.93, 0.86, 0.93, 0.93, 0.93, 0.93, 1.0, 0.93, 1.0])
```

> ⭐ **cv=10 的重點不是「提升精準度」，是看到那條分數的離散程度**——上例從 0.86 到 1.0，單跑一次可能剛好抽到 1.0 而誤以為模型很強。（課堂摘要寫成「做 10 次交叉驗證提升精準度」，措辭要修正：**交叉驗證不會讓模型變好，只會讓你對它的估計更誠實**。）

```python
# 樸素貝葉斯
from sklearn.naive_bayes import GaussianNB
clf = GaussianNB().fit(X, Y)
clf.predict([[-0.8, -1]])     # [1]

# SVM ⭐ 必須先標準化
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
clf = make_pipeline(StandardScaler(), SVC(gamma='auto')).fit(X, y)
```

> 🔴 **SVM 那行的 `make_pipeline(StandardScaler(), SVC())` 是全模組最該抄走的一行**：SVM 靠距離算 margin，**不標準化就等於讓單位大的特徵決定一切**。用 pipeline 包起來，標準化才會**只用訓練集的統計量**、不洩漏到測試集。
> → 選型判準見 `framework-预处理选型决策表`（哪些模型必須標準化）。

## 三、非監督式：分群三種

**分群（Clustering）＝只有自變數 X，沒有依變數 Y。**

| 演算法 | 機制 | 關鍵參數 |
|---|---|---|
| **階層式 Hierarchical** | 由許多單點逐漸**合併**成群（bottom-up），或由一個總群逐漸**分裂**（top-down） | `AgglomerativeClustering()` |
| **K-means** | 任意選 K 個點當質心 → 算每個樣本與質心的相似度 → 歸到最相似的群 → **重算質心 → 重複到質心不再改變** | ⚠️ **必須事先指定 `n_clusters=k`** |
| **DBSCAN** | 若點 p 的 Eps 鄰域內點數多於 MinPts，就以 p 為核心建一個簇，並把鄰域內所有點併入；沒有新點可加入時結束 | `eps`（距離）、`min_samples`（最少點數） |

```python
from sklearn.cluster import AgglomerativeClustering, KMeans, DBSCAN

AgglomerativeClustering().fit(X).labels_   # array([1,1,1,0,0,0])

km = KMeans(n_clusters=2, random_state=0).fit(X)
km.labels_            # array([1,1,1,0,0,0])
km.predict([[0,0],[12,3]])
km.cluster_centers_   # array([[10.,2.],[1.,2.]])

DBSCAN(eps=3, min_samples=2).fit(X).labels_
# array([0, 0, 0, 1, 1, -1])
```

### 🔴 一處更正：DBSCAN 的雜訊標記是 **-1**，不是 0

**課堂摘要記成「雜訊點標記為 0」，但講義 p143 的輸出明寫 `array([0, 0, 0, 1, 1, -1])`——最後那個離群點是 `-1`。**

| | 值 |
|---|---|
| 第一群 | `0` |
| 第二群 | `1` |
| ⭐ **雜訊／不屬於任何群** | **`-1`** |

> 🕳️ **為什麼這個記錯會很貴**：`0` 是一個**合法的群編號**。若照「雜訊＝0」去寫 `df[df.label != 0]`，你會**刪掉整個第一群、同時把雜訊全留著**——而且不會報錯。
> ⭐ 這是本課第三個「有數字但方向全錯且不報錯」的坑（前兩個：M6 的 Lift≈1、M8／M10 的 axis 記反）。

### ⭐ K-means vs DBSCAN 的真正分界

| | K-means | DBSCAN |
|---|---|---|
| 要先知道幾群？ | ⚠️ **要**（k 是你猜的） | **不用**（自己長出來） |
| 群的形狀 | 傾向球狀、大小相近 | 任意形狀 |
| 離群點 | **會被硬塞進某一群** | ⭐ **獨立標成 `-1`** |
| 適合 | 用戶分層（本來就想切成 N 群） | **異常偵測**（你要的正是那些 `-1`） |

## 對接

| 場景 | 用哪個 |
|---|---|
| 🎮 **異常玩家／套利偵測** | ⭐ **DBSCAN**——要的就是那些 `-1`；配 `framework-數據異常檢測抓作弊`、⚠️ `principle-异常检测只缩小范围`（**只縮小範圍，不下結論**） |
| 🎮 **流失預測／風控分類** | 羅吉斯迴歸出機率 → 配 `article-AI面試題庫2.0五大題型` 的**校準度**概念做信心分級處置 |
| 📊 **選型總表** | `framework-如何选分析算法`（聚類／回歸／分類「情境→選哪個」） |

## ⚠️ 深度一律去這些筆記（本則不重複）

| 這裡只有簡介 | 完整版在 |
|---|---|
| 線性迴歸 | `framework-回歸建模流程`（cost function／RMSE／`coef_`／房價實作） |
| 羅吉斯迴歸 | `framework-邏輯斯回歸`（Log loss／sigmoid／`C` 正則化） |
| 決策樹 | `framework-決策樹` 系列（entropy-IG）、`framework-隨機森林` |
| 樸素貝葉斯 | `framework-單純貝氏分類`（獨立假設／Laplace smoothing） |
| SVM | `framework-svm支持向量機`（margin／±1 label／kernel） |
| 評估 | ⭐ `cheat-分類模型評估混淆矩陣`（⚠️ sklearn `confusion_matrix` 左上角是 **TN 不是 TP**） |
| sklearn API 慣例 | `cheat-sklearn-api` |
| 集成 | `framework-集成學習三型`、`framework-adaboost` |
| 全場比較 | ⭐ `case-安隆案詐欺偵測`（七模型同場比，**accuracy 都在 74–83% 但三個模型 recall＝0**） |

> 📌 **兩門課的分工**：這門給**最小可跑骨架**，TibaMe L4/L5講義給**判斷與評估**。要動手先抄這裡，要解釋為什麼去那邊。

## 相關

- [MOC-資料探勘](MOC-資料探勘.md)、`MOC-tibame訓練營`
- `concept-監督式演算法`、`concept-非監督式演算法`（學程第二課的概念地圖）
- 範例程式：`Tibame_DataMiningAlgorithms.ipynb`
