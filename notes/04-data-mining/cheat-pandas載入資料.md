---
name: cheat-pandas載入資料
description: M2 載入資料速查——CSV/Excel、欄列選取與過濾、JSON 四種 orient、XML 手動解析、SQLite 用 SQLAlchemy 讀寫
metadata:
  type: cheat
book: 成為AI科學家_資料探勘
chapter: M2 載入資料
---

# 載入資料：五種來源（M2）

> 定位：ETL 的 **E（Extract）**。目標是不管來源長什麼樣，最後都變成同一個東西＝**DataFrame**。

## CSV / Excel

| 動作 | 寫法 |
|---|---|
| 讀 CSV | `pd.read_csv('path/file.csv')` |
| 讀 Excel | `pd.read_excel('path/file.xlsx')` |
| 指定分頁 | `pd.read_excel(..., sheet_name='Sheet2')`（支援多個 Sheet） |

讀進來就是 **DataFrame**（二維表格）；取一欄出來是 **Series**（一維）。兩種資料結構的關係見 `cheat-pandas-dataframe與series`。

## 欄位與列的選取／過濾

| 目的 | 寫法 | ⚠️ |
|---|---|---|
| 取單一欄（Series） | `df['欄名']` | |
| 取多欄（DataFrame） | `df[['欄1','欄2']]` | **兩層中括號** |
| 統計摘要 | `df.describe()` | 只算數值欄；⭐ 第一眼一定要跑 |
| 條件過濾 row | `df[df['欄'] > 100]` | |
| 丟掉遺失值 | `df.dropna()` | 預設整列刪，⚠️ 會刪掉大量資料 |
| 篩特定值 | `df[df['欄'].isin(['A','B'])]` | 比一連串 `\|` 好讀 |

> 更完整的組合條件（多 mask、`between()`）見 [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md)。

## JSON：四種 orient 方向

`to_json(orient=...)` / `read_json(orient=...)` 的 **orient 決定鍵值怎麼擺**，同一份資料可以有完全不同的長相：

| orient | 結構 | 什麼時候用 |
|---|---|---|
| `split` | 拆成 `{index:[], columns:[], data:[[]]}` | 最省空間，欄名只出現一次 |
| `index` | `{index: {column: value}}` | 要用 index 直接查一列 |
| `records` | `[{column: value}, ...]` | ⭐ **最常見**，一列一物件；API 回傳幾乎都長這樣 |
| `table` | 含 schema 描述 | 要保留型別資訊時 |

🕳️ **接別人的 API 前先確認 orient**——`records` 直接 `pd.DataFrame(data)` 就好，`split` 硬灌會整個錯位。動態 API 的實際處理見 `framework-json與動態網頁`。

## XML：沒有現成函數

**pandas 沒有可靠的一鍵 read_xml 路徑（本課採手動解析）**，做法是：

```
建立空白 DataFrame → 逐一走訪 XML 節點 → 把節點資料一列一列填進去
```

⚠️ 這是本模組唯一「要自己寫迴圈」的來源格式；填列的寫法（`df.loc[n] = [...]`）見 `cheat-pandas-dataframe與series`。

## 資料庫（SQLite）

```python
from sqlalchemy import create_engine
engine = create_engine('sqlite:///mydb.db')

df.to_sql('table_name', engine)          # 寫入
df2 = pd.read_sql_table('table_name', engine)   # 讀回
```

三件套＝**`create_engine()` 建連線 → `to_sql()` 寫 → `read_sql_table()` 讀**。換資料庫（MySQL/Postgres）只改連線字串，後面兩行不變。

## 失敗模式

- ❌ 讀完不 `describe()` 就開始算——型別被讀成 object、數字混文字都看不出來。
- ❌ 直接 `dropna()` 圖方便——先確認遺失是「隨機缺」還是「有意義的缺」，見 [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md)。
- ⚠️ 路徑問題：Colab 要先掛載 Google Drive，見 [reference-課程環境與工具](reference-課程環境與工具.md)。

## 相關

[MOC-資料探勘](MOC-資料探勘.md) · [concept-資料探勘與kdd流程](concept-資料探勘與kdd流程.md) · `cheat-pandas過濾器與csv`
