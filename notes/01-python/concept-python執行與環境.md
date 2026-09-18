---
name: concept-python執行與環境
description: Python 語言定位（高階/直譯/動態）、執行流程、Anaconda 虛擬環境、Jupyter/其他開發環境速查
metadata:
  type: concept
book: 快速闖關Python語法世界
chapter: M1-2,M30
---

# Python 執行模型與開發環境

## Python 是什麼（一句定位）

| 維度 | Python 落點 | 對照 |
|---|---|---|
| 階層 | 高階語言（語法近人類邏輯） | 低階＝機器碼/組合語言 |
| 翻譯方式 | **直譯式**（邊執行邊翻譯） | 編譯式＝C/C++/Java 先 compile |
| 型別 | **動態型別**（執行期自動判型、可變） | 靜態＝C/Java 須先宣告型別 |
| 標籤 | 跨平台、可讀性高、第三方套件豐富、腳本語言、**膠水語言** | — |

執行流程：原始碼 `.py` → 直譯器逐行翻成位元組碼 → 由 Python VM 執行。

## Anaconda 虛擬環境

- 為**不同專案各開一個 env**，避免套件版本衝突（Project A 用 Py2.x、B/C 用 Py3.x 各自獨立）。
- 安裝套件兩條路：`pip install <pkg>` 或 `conda install <pkg>`。
- 本機實況（訓練營）：env = `ds`（Python 3.13）；建/用見 [`docs/environment.md`](../../docs/environment.md)。

```bash
conda create -n myenv python=3.11   # 建環境
conda activate myenv                # 進環境
pip install pandas                  # 裝套件
```

## Jupyter Notebook 速查

- 互動式網頁計算環境，副檔名 `.ipynb`，可跑 Python。
- 兩種 cell：**Code cell**（寫程式）、**Markdown cell**（寫筆記，可含文字/圖/影片，能轉 html）。
- Code cell 狀態符號：

| 符號 | 意義 |
|---|---|
| `In [ ]` | 尚未執行 |
| `In [*]` | 執行中 |
| `In [num]` | 已執行（num＝執行序號） |

- 執行：工具列 Run 或 **`Shift + Enter`**。
- ⚠️ 使用者實務用 **VSCode 跑 notebook**（非瀏覽器 Jupyter），給範例優先 `.ipynb` 或 `# %%` 分段 `.py`。

## 其他開發環境（M30）

| 環境 | 性質 |
|---|---|
| Jupyter Notebook | 互動、資料科學最常用 |
| **PyCharm** | JetBrains 完整 IDE（debug/重構強），jetbrains.com/pycharm |
| **repl.it** | 雲端、免安裝、開瀏覽器即用 |
| VSCode | 使用者本職環境（+ Python/Jupyter 擴充） |

延伸：型別轉換與物件本質見 [cheat-資料型別總覽](cheat-資料型別總覽.md)；語法保留字見 [cheat-基礎語法](cheat-基礎語法.md)。
