---
name: cheat-pandas清洗與轉換
description: M3 資料清洗(一)速查——邏輯 mask 組合過濾、文字型別與字串操作、時間序列 resample、遺失值偵測與三種填補法
metadata:
  type: cheat
book: 成為AI科學家_資料探勘
chapter: M3 資料清洗與轉換（一）
---

# 資料清洗與轉換（一）：篩選／文字／時間／遺失值（M3）

> 定位：ETL 的 **T（Transform）上半場**。四件事：**挑對的列、處理文字、處理時間、補洞**。

## 1. 資料篩選：mask 思維

核心觀念＝**條件式先產生一串 True/False（mask），再拿 mask 去切 DataFrame**。

| 目的 | 寫法 | ⚠️ |
|---|---|---|
| 單條件 | `df[df['age'] > 30]` | |
| **AND** | `df[(df['a']>1) & (df['b']<5)]` | ⭐ 每個條件都要**自己的括號**，且用 `&` 不是 `and` |
| **OR** | `df[(df['a']==1) \| (df['b']==2)]` | 同上，用 `\|` 不是 `or` |
| 列舉值 | `df[df['city'].isin(['台北','高雄'])]` | |
| 區間 | `df[df['score'].between(60, 90)]` | 預設**含頭含尾** |

🕳️ **最常見的錯**＝忘記括號或用了 Python 的 `and`／`or` → 直接 `ValueError: truth value of a Series is ambiguous`。

## 2. 文字資料

- pandas **預設把文字存成 `object` 型別**；可明確指定 `dtype='string'` 拿到真正的字串型別。
- 字串操作走 `.str` 存取器：`df['col'].str.split('-')`、`df['col'].str.cat()`（串接）等。

## 3. 時間資料

支援三種時間類型：**datetimes（時間點）／timedelta（時間差）／timespan（時間區間）**。

| 動作 | 寫法／重點 |
|---|---|
| 轉時間型別 | `pd.to_datetime(df['date'])` |
| 建時間序列 | `pd.date_range(start, periods, freq)` |
| **設為 index** | `df.set_index('date')` — ⭐ 做時間分析前的必要一步 |
| **重採樣** | `df.resample('M').sum()` — 改變頻率（日→月、秒→分） |

⭐ **`resample()` 是本模組最有價值的一招**：把高頻資料壓成低頻，等於用時間軸做 groupby。

⚠️ 但**時間序列不適合複雜商業環境**——用「上線前 vs 上線後」比較會被外部變化污染，這是 A/B 必須並行的理論依據，見 `principle-时间序列不适合复杂商业环境`。

## 4. 遺失值：偵測與填補

**偵測**：`df.isnull()` / `df.notnull()`（配 `.sum()` 看每欄缺幾筆）。

**填補三招**：

| 方法 | 寫法 | 適用 |
|---|---|---|
| **固定值／前後值** | `df.fillna(0)`、`fillna(method='ffill')` | 類別欄、狀態欄（沿用上一筆） |
| **替換** | `df.replace(舊, 新)` | 缺值被寫成 `-1`／`'N/A'` 這種偽遺失值 |
| **內插** | `df.interpolate()` | ⭐ **連續數值**（感測器、股價、時間序列），線性補中間的洞 |

🕳️ **選錯填補法比不填更糟**：對類別欄做 `interpolate()`、對趨勢資料一律 `fillna(0)`，都會製造假訊號。選型判準見 `framework-预处理选型决策表`。

## 失敗模式

- ❌ **無條件 `dropna()`**——遺失本身可能就是訊號（沒填＝不想填）。
- ❌ 補完值不記錄補了多少——後面的統計量全被稀釋而不自知。

## 相關

[MOC-資料探勘](MOC-資料探勘.md) · [framework-離群值與資料轉換](framework-離群值與資料轉換.md) · [cheat-pandas載入資料](cheat-pandas載入資料.md)
