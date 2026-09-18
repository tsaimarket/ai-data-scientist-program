# Python 完整教學（M1–M30）

《快速闖關 Python 語法世界，程式實作不頭痛》全 30 模組的**線性教學版**。

- 與 `notes/` 的分工：`notes/` 是**查表**（忘了語法回來翻），這份是**從頭讀一遍**（概念→語法→可跑範例→坑）。
- 來源：課程講義 PDF（324 頁）＋ Notion 課堂會議筆記 9 篇（講師口述補充與習題檢討）。
- 環境：conda env `ds`，VSCode 跑 `.ipynb`；MySQL 用本機 Docker `mysql`。

| 段 | 模組 | 主題 |
|---|---|---|
| Part 0 | M1–M2、M30 | 語言概念與開發環境 |
| Part 1 | M3–M7 | 基礎語法、運算元、資料型別 |
| Part 2 | M8–M9 | 流程控制與迴圈 |
| Part 3 | M10 | 字串格式化與正規表達式 |
| Part 4 | M11–M13 | 函數、lambda、遞迴與 DP |
| Part 5 | M14–M15 | 類別與物件導向 |
| Part 6 | M16–M24 | 例外、檔案、內建模組、第三方模組、執行緒 |
| Part 7 | M25–M27 | SQLite 與 MySQL |
| Part 8 | M28–M29 | Tkinter GUI |

---

# Part 0 · 語言概念與開發環境（M1–M2、M30）

## M1 什麼是程式語言

程式語言 = 一套有**語法、詞彙、含義**的語言，用來叫電腦做事。

**兩組分類軸，記住 Python 落在哪一格：**

| 軸 | 兩端 | 差別 | Python |
|---|---|---|---|
| 抽象層級 | 低階（機器碼、組合語言） vs **高階**（Python/C++/Java） | 低階貼近機器不好讀；高階貼近人類自然語言 | 高階 |
| 翻譯方式 | 編譯式 Compiled（C/C++/Java） vs **直譯式 Interpreted** | 編譯式整份翻完再跑；直譯式逐行翻逐行跑（像隨行翻譯） | 直譯式 |

**Python 六個特色**：可讀性高、跨平台、第三方套件豐富、直譯式、腳本語言（自動化重複工作）、膠水語言（可與其他語言混用）。設計理念寫在 The Zen of Python（`import this` 可印出來）。

**執行流程**：原始碼 Source Code → 直譯器 Interpreter 逐行翻譯 →（結合第三方套件）→ 輸出結果。

資料科學三大生態系套件先認得名字：`NumPy`（數值運算）、`Pandas`（表格資料，類 Excel）、`Matplotlib`（統計圖表）。

## M2 開發環境建置（Anaconda + Jupyter）

**為什麼用 Anaconda**：專精科學計算的免費開源發行版，幫你把套件管理簡化，而且能開**虛擬環境**。

**虛擬環境的意義**：隔離不同專案的套件與版本，避免衝突；上課時也讓全班環境與講師一致。`base` 是本機預設環境，**不建議直接用**。

```
安裝 Anaconda → 全程「下一步」預設值即可 → 開 Anaconda Navigator 確認成功
Environment → Create → 命名（如 PythonCosmos）→ 選 Python 版本
```

**開虛擬環境的 Command Line**（安裝套件一定要在這裡開）：

```
Anaconda Navigator → Environment → 環境旁的綠色/白色箭頭 → Open Terminal
```

> ⚠️ **最常見的錯**：在系統的 cmd 裡 `pip install`，裝到別的環境去。判斷方法：虛擬環境的 terminal **前面括號會顯示環境名稱**。

```bash
pip install pandas          # 裝套件
conda install pandas        # 或用 conda 裝
```

裝完新套件後，Jupyter 要 **Kernel → Restart** 才吃得到。

**Jupyter Notebook**：網頁式互動計算環境，副檔名 `.ipynb`。

| 概念 | 說明 |
|---|---|
| Code Cell | 寫並執行 Python 程式碼 |
| Markdown Cell | 寫筆記（文字、圖片、影片），不會被當程式執行 |
| 執行 | 按 Run，或快捷鍵 `Shift + Enter`（執行並跳下一格） |
| `In[3]` | 已執行完成（數字是執行順序） |
| `In[ ]` | 尚未執行 |
| `In[*]` | **執行中**——若一直卡星號，多半是無窮迴圈 → Kernel → Restart |

**檔案位置的坑**：Jupyter 預設不好切到 D 槽。建議 Anaconda 與課程檔案都放 **C 槽**，做完再複製/剪下搬到 D 槽。

**備援方案 Google Colab**：本機裝不起來就用雲端。操作類似 Jupyter（「加程式碼」「加文字」），可用「檔案 → 上傳筆記本」把本機 `.ipynb` 丟上去。

> ⚠️ Colab 的程式碼存在 Google 雲端，**長時間閒置會斷線、可能掉資料**。養成習慣：檔案 → 下載 → 下載 `.ipynb`。

## M30 其他開發環境

| 工具 | 說明 |
|---|---|
| **PyCharm** | 主流 Python IDE，下載 **Community**（免費開源）版；New Project 時要指定 Python Interpreter。首次建專案可能跳 Permission Denial，按 OK 略過即可 |
| **Replit** | 雲端 IDE，免安裝，瀏覽器直接寫直接跑，結果顯示在右側，支援上傳下載 |
| **VSCode** | （本訓練營實際使用）Python + Jupyter 擴充，右上角 Select Kernel 選 `ds` |

---

# Part 1 · 基礎語法與資料型別（M3–M7）

## M3 基礎語法

### keyword（保留字）

Python 內建、有特殊用途，不能拿來當變數名。

```python
import keyword
keyword.kwlist                # 列出全部保留字
keyword.iskeyword("for")      # True
keyword.iskeyword("IF")       # False（大小寫有別）
```

### identifier（識別字）

變數、函數、類別的名字。規則：

- 只能用 `A-Z`、`a-z`、`0-9`、底線 `_`
- **第一個字元不可為數字**
- 大小寫有別
- 強烈建議不要用 keyword

```python
"ab10c".isidentifier()   # True
"9abc".isidentifier()    # False
```

> ⚠️ **課堂特別強調的缺陷**：`isidentifier()` 對保留字也回傳 `True`（`"for".isidentifier()` → `True`），所以合法性檢查要**兩個一起判**：
> ```python
> def is_valid(name):
>     return name.isidentifier() and not keyword.iskeyword(name)
> ```

### literal（字面值）

| 類別 | 寫法 |
|---|---|
| 數字 | 十進 `30`、二進 `0b11001`、八進 `0o156`、十六進 `0x12` |
| 字串 | `'單引號'` 或 `"雙引號"` |
| 逃脫字元 | `\n` 換行（先認得這個就夠）、`\t`、`\\` |
| 布林 | `True` / `False` |

### 程式結構四件事

```python
a = 10              # 敘述：一行一指令；= 是賦值（右邊寫入左邊）

# 單行註解
'''
多行註解（三個單引號包住）
'''

total = 1 + 2 + \
        3 + 4       # 多行敘述：行尾反斜線接下一行

if a > 5:
    print("大")     # 縮排：4 個空白代表程式區塊
```

> ⚠️ **不可混用空白鍵與 Tab**，否則 `IndentationError`。

### print 與 input

```python
print("Hello Python")
print("A", end="")      # end 控制結尾字元，預設 "\n"；設 "" 就不換行
print("B")              # → AB

print("{} 今年 {} 歲".format("Jason", 19))   # format：{} 預留空位
```

```python
name = input("請輸入姓名：")        # ⚠️ input 回傳的一定是「字串」
age  = int(input("請輸入年齡："))   # 要算數就得轉型
h    = float(input("請輸入身高："))
```

**Practice（M3）**：BMI 計算器（`weight / height**2`）、圓面積（`Pi * r * r`，輸入轉 `float`）、攝氏轉華氏（`c * 9/5 + 32`）。→ 解法見 `notes/drill-習題解法彙編`。

## M4 變數與運算元

### 變數 = 記憶體空間；每個物件有三個屬性

| 屬性 | 取得方式 | 說明 |
|---|---|---|
| ID | `id(a)` | 物件在記憶體的位置 |
| Type | `type(a)` | 型別 |
| Value | `a` | 數值 |

```python
a = "Hello"
b = "Hello"
id(a) == id(b)     # True —— 兩者指向同一塊記憶體
```

**多重賦值**：

```python
a = b = c = 5          # 從右到左
x, y, z = 1, 2, 3      # 一次拆開賦值
```

**動態型別**：Python 會自動偵測與轉換變數型別；C/C++/Java 是靜態型別，得先宣告。

### 七類運算元

**① 算術** `+ - * /` 加上三個要記的：

```python
10 % 3    # 1   取餘數
3 ** 3    # 27  次方
10 // 3   # 3   整數除法（無條件捨去）
```

**② 比較**（回傳一定是 `True`/`False`）：`==` `!=` `>` `<` `>=` `<=`

> ⚠️ 初學者頭號地雷：`=` 是**賦值**，`==` 才是**比較**。

**③ 賦值**：`x += 3` 等價 `x = x + 3`；同理 `-=` `*=` `/=` `//=` `%=` `**=`

**④ 邏輯**：

| 運算 | 規則 |
|---|---|
| `and` | 兩邊都 True 才 True |
| `or` | 任一邊 True 就 True |
| `not` | True/False 對調 |

**⑤ 成員**：`x in y` — x 是否存在於 y 中

**⑥ 身份**：`is` — 兩變數是否指向記憶體同一位置

```python
a = [1, 2]
b = [1, 2]
a == b     # True （值相同）
a is b     # False（不同記憶體位置）
```

**⑦ 位元**（轉二進位後逐位運算）：

```python
6 & 3      # AND
6 | 3      # OR
10 << 2    # 40  左移＝乘 2^n（後面補 0）
10 >> 2    # 2   右移＝除 2^n（捨去右邊位元）
```

## M5 數值型別與布林型別

**Mutable vs Immutable**（貫穿全課的概念）：

| 類別 | 型別 | 定義後可否修改元素 |
|---|---|---|
| Mutable 可變 | `list`、`set`、`dict` | ✅ 可 |
| Immutable 不可變 | `str`、`tuple`、數值型別 | ❌ 不可 |

**數值三型**：

```python
i = 100          # int   支援 0b / 0o / 0x 表示法
f = 22.5         # float 科學記號 18.3e9 = 18.3 × 10^9
c = 1 + 2j       # complex 虛數，j 代表根號 -1
type(f)          # <class 'float'>
```

**布林**：只有 `True` / `False`，常出現在條件比較。`print(5 > 3)` → `True`。

## M6 序列型別（字串、list、tuple）

### 字串 String

```python
s = "Hello Python"
s[0]        # 'H'   索引從 0 開始
s[-1]       # 'n'   負數從右邊數
s[2:]       # 'llo Python'      切片（Slicing）
s[:3]       # 'Hel'             ⚠️ 不含結尾 index
s[1:-1:2]   # 步進為 2
```

```python
"abc" + "def"      # 黏接
"ab" * 3           # 重複 → 'ababab'
```

常用內建方法：

| 方法 | 作用 |
|---|---|
| `.lower()` / `.upper()` | 轉小寫 / 大寫 |
| `.replace(a, b)` | 取代 |
| `.split()` | 依分隔符切成 list |
| `.strip()` | 去除頭尾空白與換行符 `\n` |
| `"".join(list)` | 把 list 元素黏成字串 |
| `len(s)` | 長度 |

### 列表 List

有序、**可變**、允許重複與混型別，用 `[]` 建立。索引/切片語法與字串相同。

| 方法 | 作用 |
|---|---|
| `.append(x)` | 加**一個**元素到最後 |
| `.extend(lst)` | 把另一個 list 的**各元素**合併進來 |
| `.insert(i, x)` | 插入到指定位置 |
| `.pop()` | 移除並回傳最後一個 |
| `.remove(x)` | 移除指定值 |
| `.reverse()` | 反轉 |
| `.sort()` | 排序 |
| `del lst[i]` | 刪除指定索引 |

> ⚠️ **課堂重點**：`a.append(b)` 會把整個 `b` 當成**一個元素**塞進去（變巢狀 list）。要合併請用 `.extend()`。

### 元組 Tuple

有序、**不可變**，用 `()` 建立，其餘操作與 list 相同。

```python
t = (1, 2, 3)
t[0] = 9    # ❌ TypeError: 'tuple' object does not support item assignment
```

## M7 集合、字典與型別轉換

### 集合 Set

無序、**不允許重複**，用 `{}` 建立。

```python
s = {1, 2, 3}
s.add(4)              # 加一個
s.update([5, 6])      # 加多個
s.remove(1)           # 移除指定
s.pop()               # 隨機移除一個（因為無序）

{1,2,3} & {2,3,4}     # {2, 3}       交集
{1,2,3} | {2,3,4}     # {1,2,3,4}    聯集
```

> 💡 **實用技巧**：`list(set(mylist))` 一行去除重複元素。

### 字典 Dictionary

以 Key-Value Pair 儲存，用 `{}` 加冒號建立。

```python
d = {"Jason": 19, "Mary": 22}
d["Jason"]          # 19       取值
d["Tom"] = 30       # 新增（key 不存在）或更新（key 已存在）
d.keys()            # 所有 key
d.values()          # 所有 value
d.pop("Mary")       # 移除指定 key
```

### 型別轉換 Type Casting

用與型別同名的函數轉換：

```python
int("100")        # 100
float("1000")     # 1000.0
str(123)          # '123'
list("abc")       # ['a', 'b', 'c']   ⚠️ 字串轉 list 會把每個字元拆開
tuple([1, 2])     # (1, 2)
set([1, 1, 2])    # {1, 2}
```

**Practice（M4–M7）**：六位數幸運數字判斷、海倫公式算三角形面積、list 切片與 `del`、字典新增 KV、字串操作綜合題。→ 解法見 `notes/drill-習題解法彙編`。

---

# Part 2 · 流程控制與迴圈（M8–M9）

## M8 條件判斷

**先畫流程圖再寫程式**（課程要求的習慣）：

| 符號 | 意義 |
|---|---|
| 箭頭 | 流程方向 |
| 長方形 | 程序/處理 |
| 菱形 | 條件判斷 |
| 平行四邊形 | 輸入 / 輸出 |

```python
score = 75

if score >= 90:          # 條件為真才執行縮排區塊
    grade = "A"
elif score >= 60:        # 可加無限多個 elif
    grade = "B"
else:                    # 前面都不成立
    grade = "C"
```

| 寫法 | 行為 |
|---|---|
| `if / elif / else` | **強迫多選一**，只有一個分支會跑 |
| 多個獨立 `if` | 各自判斷，可能多個都跑、也可能都不跑 |

**複合條件**：用 `and` / `or` / `not` 組合。注意 Python 把**非 0 的數字視為 True**。

**巢狀**：if 區塊裡再放 if/else，用縮排層級區分從屬關係。

**Practice（M8）**：
- 交換兩變數 → 需借助暫存變數 `temp`（或 Python 特有的 `a, b = b, a`）
- 判斷母音 → `if ch in ['a','e','i','o','u']`（比一串 `or` 乾淨）
- 分數轉等級 → if/elif/else
- 判斷字元類別 → 用 `ord()` 取 ASCII 碼做區間比較

## M9 迴圈

### while

條件為真就一直跑，每跑完一輪回頭再檢查條件。

```python
total, i = 0, 1
while i <= 100:
    total += i
    i += 1
```

| 關鍵字 | 作用 |
|---|---|
| `break` | **立刻跳出**整個迴圈；常搭配 `while True:` 無窮迴圈使用 |
| `continue` | **跳回條件判斷**，略過本輪剩下的程式碼 |

```python
i = 0
while i < 100:
    i += 1
    if i % 2 == 1:
        continue        # 奇數跳過
    total += i          # 只加偶數
```

> ⚠️ Jupyter 的 cell 一直顯示 `In[*]` → 很可能進了無窮迴圈，Kernel → Restart。

### for

用來迭代序列資料（list / tuple / dict / string）。

```python
for i in range(5):              # 0,1,2,3,4
for i in range(2, 10, 2):       # 2,4,6,8（start, end不含, step）

for idx, val in enumerate(mylist):   # 同時取索引與值
    print(idx, val)

for k in mydict:                # 迭代 key
for v in mydict.values():       # 迭代 value
```

### 巢狀迴圈

外層跑一圈，內層完整跑一輪。

```python
for i in range(5):
    for j in range(5):
        print("*", end="")
    print()          # ⚠️ 換行的 print() 要放在「外層」迴圈結尾
```

**Practice（M9）**：金字塔（第 i 層空白 `level-i-1` 個、星號 `2i+1` 個）、階乘、統計奇偶數個數、`continue` 跳號、找 1500–2700 間同時被 7 與 5 整除的數、字串反轉、九九乘法表、list 加總、找最大值（先假設第一個最大再逐一比較覆寫）、去重複、質數判斷（`isPrime` 旗標 + 2 到 n/2 試除 + 找到就 `break`）、字元頻率統計（用 dict）、找最長單字、迴文判斷。→ 解法見 `notes/drill-習題解法彙編`。

---

# Part 3 · 字串格式化與正規表達式（M10）

## 字串格式化

### `.format()` 方法

```python
"{} 和 {}".format("A", "B")                     # 依序帶入
"{1} 和 {0}".format("A", "B")                   # 指定第幾個參數
"緯度 {latitude}".format(latitude=25.03)        # 關鍵字帶入
```

**對齊與進位**：

| 格式 | 效果 |
|---|---|
| `{:10d}` | 預留 10 格，十進位 |
| `{:>10}` / `{:<10}` / `{:^10}` | 靠右 / 靠左 / 置中 |
| `{:#b}` `{:#o}` `{:#x}` | 二 / 八 / 十六進位（含前綴） |
| `{:,}` | 千分位逗號 |
| `{:.2%}` | 百分比到小數兩位 |

```python
from datetime import datetime
datetime.now().strftime("%Y%m%d%H%M%S")     # 20260731170000
```

### `%` 舊式格式

`%10d`（整數）、`%10f`（浮點，預設六位小數）、`%10.2f`（指定小數位）、`%10s`（字串）。

> 💡 現代 Python 更常用 **f-string**：`f"{name} 今年 {age} 歲"` — 講義偏舊，但三種都看得懂比較好。

## 正規表達式 Regular Expression

**基本 pattern**：

| 符號 | 意義 |
|---|---|
| `?` | 前一字元出現 0–1 次 |
| `+` | 出現 1 次以上 |
| `*` | 出現 0 次以上 |
| `{n,m}` | 出現 n 到 m 次 |
| `\d` / `\D` | 數字 / 非數字 |
| `\w` / `\s` | 英數底線 / 空白 |
| `^` | 字串起始 |
| `.` | 任意字元（`\.` 才是真正的點） |
| `\|` | 或 |
| `()` | 群組擷取 |

**三個核心函數**：

```python
import re

re.match(r"\d+", "123abc")      # 從「第一個字元」開始比對，不符就失敗
re.search(r"\d+", "abc123")     # 不限位置，任意處找到即可（較寬鬆）
re.findall(r"\d+", "a1b22c333") # ['1', '22', '333'] 找出所有符合
```

```python
# 擷取 email 的小老鼠前後兩段
m = re.search(r"(\w+)@(\w+\.com)", "service@gmail.com")
m.group(1)    # 'service'
m.group(2)    # 'gmail.com'
```

> 💡 **課堂建議**：寫 pattern 時用線上工具（regex101 之類）即時測試，符合處會綠底標示，旁邊有 cheat sheet 可查。

---

# Part 4 · 函數與遞迴（M11–M13）

## M11 自訂函數與內建函數

```python
def greet(name, greeting="Hello"):     # greeting 是預設參數
    return f"{greeting}, {name}"       # return 一執行就立刻跳出函數

greet("Tom")                # 'Hello, Tom'
greet("Tom", "Hi")          # 'Hi, Tom'
```

> 函數沒寫 `return` 時，預設回傳 `None`。

**四種參數**：

| 類型 | 寫法 | 說明 |
|---|---|---|
| 一般參數 | `def f(a, b)` | 依位置傳入 |
| 預設參數 | `def f(a, b=10)` | 沒傳就用預設值 |
| `*args` | `def f(*args)` | 不定長度，收成 **tuple** |
| `**kwargs` | `def f(**kwargs)` | 不定長度鍵值，收成 **dict** |

**主程式慣例**：

```python
def main():
    ...

if __name__ == "__main__":     # 判斷是否為「直接執行」而非被 import
    main()
```

**常用內建函數**：`abs()` 絕對值、`pow()` 次方、`sum()` 加總、`len()` 長度、`int()`/`str()`/`float()` 轉型、`ord()` 轉 ASCII、`eval()` 對四則運算字串求值。

**全域 vs 區域變數**：

```python
count = 0

def add():
    global count      # 沒宣告 global 就不能改到外面的 count
    count += 1
```

函數內定義的變數是**區域變數**，離開函數就消失。

## M12 匿名函數 Lambda

一行寫完的小函數，常搭配 `filter` / `map` / `reduce`。

```python
f = lambda a: a * 2
g = lambda x, y: x * y

nums = [1, 2, 3, 4, 5]

list(filter(lambda x: x % 2 == 1, nums))    # [1,3,5]  篩選
list(map(lambda x: x ** 2, nums))           # [1,4,9,16,25] 逐一套用

from functools import reduce
reduce(lambda a, b: a + b, nums)            # 15  逐一合併
```

> 💡 **為什麼要學**：MapReduce 是大數據分析與分散式運算的基礎思路，後面資料處理會一直遇到。

## M13 遞迴與動態規劃

**遞迴 Recursion** = 函式呼叫自己。**必須設終止條件（base case）**，否則無限遞迴。

```python
def factorial(n):
    if n == 0:            # base case
        return 1
    return n * factorial(n - 1)
```

> ⚠️ Python 預設遞迴上限 **3000 次**，超過會 `RecursionError`。可調整（但通常代表該換寫法）：
> ```python
> import sys
> sys.setrecursionlimit(5000)
> ```

**費氏數列**：`F(n) = F(n-1) + F(n-2)`，`F(0)=0, F(1)=1`。純遞迴寫起來漂亮但**大量重複計算**，n 一大就爆。

**動態規劃 DP**：把子問題的結果存進 `memo`，避免重複算。

```python
def fib_dp(n):
    memo = [0, 1] + [0] * (n - 1)
    for i in range(2, n + 1):
        memo[i] = memo[i-1] + memo[i-2]     # 直接取用算好的值
    return memo[n]
```

**Practice（M11–M13）**：`multiply`、`factorial`、大小寫字母計數、1–20 平方、平均值、組合公式 C(n,r)、找子集合數、兩 list 取交集、連續三判斷、Blackjack 函數、數字三角形 pattern。→ 解法見 `notes/drill-習題解法彙編`。

---

# Part 5 · 類別與物件導向（M14–M15）

Python 是物件導向語言，**所有資料皆為物件**（string、list、tuple 都是）。

**OOP 三大特徵**：封裝 Encapsulation、繼承 Inheritance、多型 Polymorphism。
**好處**：提升軟體的重用性、擴充性、維護性。

## M14 class 基本語法

```python
class BankAccount:            # ⚠️ 類別名首字母必須大寫（慣例）
    def __init__(self, owner, balance):   # 建構子，創建物件時「自動執行」
        self.owner = owner                # self.xxx 定義屬性 attribute
        self.balance = balance

    def show(self):           # method 的第一個參數固定是 self
        print(f"{self.owner}: {self.balance}")

a = BankAccount("Jason", 1000)    # 建立物件 → 自動跑 __init__
b = BankAccount("Mary", 5000)     # 多個物件各自獨立
a.show()
```

## M15 三大特徵

### ① 封裝 Encapsulation

**目的**：防止外部直接亂改內部變數（例如有人直接把 `balance` 設成一萬）。
**做法**：attribute 或 method 名稱前加**兩個底線** `__` 使其私有化。

```python
class BankAccount:
    def __init__(self):
        self.__balance = 0            # 私有屬性

    def __calculateRate(self):        # 私有方法
        return self.__balance * 0.01

    def deposit(self, amount):        # 只能透過對外的 method 操作
        self.__balance += amount

a = BankAccount()
a.__balance = 10000     # 改不到真正的內部變數（外部無法直接存取）
```

### ② 繼承 Inheritance

「爸爸有的，小孩都能用」——子類別繼承父類別所有 attribute 與 method。

```python
class Vehicle:
    def describe(self):
        print("我是交通工具")

class Car(Vehicle):        # 語法：class 子類別(父類別)
    pass

Car().describe()           # 繼承來的
```

**多重繼承**：`class Son(Father, Uncle)` 可同時繼承多個父類別。

> ⚠️ **課堂重點**：多個父類別有**同名 method** 時，以**第一個繼承的類別**為優先。

### ③ 多型 Polymorphism

子類別可**覆寫 Override** 父類別的同名函數，讓行為不同。

```python
class Shape:
    def __init__(self, color, fill):
        self.color = color
        self.fill = fill
    def area(self):
        return 0

class Circle(Shape):
    def __init__(self, color, fill, radius):
        super().__init__(color, fill)     # 強制呼叫父類別的 __init__，保留初始化內容
        self.radius = radius              # 再新增自己的屬性
    def area(self):                       # 覆寫
        return self.radius ** 2 * 3.14
    def perimeter(self):                  # 也可新增自己的方法
        return 2 * self.radius * 3.14
```

**Practice（M14–M15）**：`Circle` 算面積、`IOStream` 輸入轉大寫、`Song` 逐行印歌詞、`Vehicle` 父類別衍生兩台車、`Temperature` 攝氏華氏互轉、`Time` 時間相加含進位、`Shape` → `Circle`/`Rectangle` 繼承體系。→ 解法見 `notes/drill-習題解法彙編`。

---

# Part 6 · 例外、檔案、模組、執行緒（M16–M24）

## M16 例外處理與檔案 I/O

### try / except

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:     # 可指定例外類型，e 是 Python 內建錯誤訊息
    print("除零錯誤：", e)
else:
    print("沒出錯才跑這裡")
finally:
    print("不管有沒有出錯都會跑")
```

**主動製造例外**：

```python
if len(password) < 8:
    raise Exception("密碼長度不足")     # raise：人工拋出例外

assert x > 0, "x 必須為正數"           # assert：條件「不滿足」時才報錯
```

### CSV 格式

每一行一筆資料，欄位之間用逗號分隔。用記事本/Notepad++ 建立、存成 `.csv`，Excel 開起來就是表格。

### 檔案讀寫

```python
f = open("data.txt", "r", encoding="utf-8")
content = f.read()          # 全部內容
f.close()                   # 記得關

f.read(10)                  # 前 10 個字元
f.readlines()               # 每行一個元素的 list（元素末尾含 \n）
```

**推薦寫法**（離開區塊自動關檔，不必手動 `close()`）：

```python
with open("data.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())      # strip() 去掉行尾換行符
```

| mode | 意義 |
|---|---|
| `r` | 讀取 |
| `w` | **覆蓋**寫入（原內容會消失） |
| `a` | 附加，寫在檔案末尾 |

**Practice（M16）**：計算檔案字數（先把逗號換成空白 → `.split()` → `len()`）、計算行數（`len(f.readlines())`）、找最長英文單字。

## M17–M19 內建模組（第一組）

### datetime

```python
from datetime import datetime as dt, timedelta

now = dt.now()
now.year, now.month, now.day
now + timedelta(hours=3)          # 時間加減

start = dt.now(); ...; end = dt.now()
print(end - start)                # 算程式執行時間
```

### math

`sqrt` 開根號、`ceil` 無條件進位、`floor` 無條件捨去、`sin`/`cos`、`pi`、`log`、`exp`。

### random

```python
import random
random.random()                        # 0~1 之間浮點亂數
random.randrange(1, 100)               # 區間整數
random.shuffle(mylist)                 # 洗牌（就地修改）
random.sample(mylist, 3)               # 抽 3 個
random.choices(mylist, weights=[...])  # 依權重隨機選
random.seed(42)                        # 固定種子 → 每次結果一致，方便 debug
```

### os

```python
import os
os.getcwd()                  # 當前工作目錄
os.listdir(path)             # 列出目錄下所有檔案
os.path.exists(p)            # 路徑是否存在
os.path.isfile(p)            # 是否為檔案
os.rename(old, new)          # 改檔名
os.remove(p)                 # 刪檔
os.makedirs(p)               # 建資料夾
```

### shutil

```python
import shutil
shutil.copy(src, dst)        # 複製單一檔案
shutil.copytree(src, dst)    # 複製整個資料夾
shutil.move(src, dst)        # 移動
shutil.rmtree(path)          # ⚠️ 刪除整個資料夾（不可復原，小心用）
```

### json

JSON 由大括號與方括號構成，是網路資料傳輸最常見的格式。

```python
import json
json.loads(json_str)         # JSON 字串 → Python dict
json.dumps(py_obj)           # Python 物件 → JSON 字串
json.load(f)                 # 從檔案讀 JSON → Python 物件
json.dump(obj, f)            # 寫入檔案
```

### time / sys / zipfile

```python
import time
time.time()                        # 自 1970/1/1 至今的秒數
time.sleep(3)                      # 暫停 3 秒
time.strftime("%Y-%m-%d", time.localtime())

import sys
sys.argv                           # 接收外部參數（Jupyter 內較難示範，建議自行研究）

import zipfile
z = zipfile.ZipFile("a.zip", "r")
z.printdir()                       # 列出壓縮檔內容
z.extractall()                     # 解壓縮
```

## M20 logging 日誌

**用途**：取代開發時滿地的 `print`。可按嚴重性分等級管理，部署時一行就關掉不重要的輸出。

| 等級 | 數值 |
|---|---|
| DEBUG | 10 |
| INFO | 20 |
| **WARNING** | 30 ← 預設只顯示這級以上 |
| ERROR | 40 |
| CRITICAL | 50 |

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,                                  # 調整顯示等級
    format="%(asctime)s [%(levelname)s] %(message)s",     # 加時間戳與等級
    filename="mylog.log",                                 # 寫入檔案
    filemode="w"
)

logging.debug("除錯訊息")
logging.warning("警告")
logging.error("錯誤")
```

## M21–M22 第三方模組

### pip 指令

```bash
pip install jieba                    # 安裝
pip install pandas==1.5.3            # 指定版本
pip install -U requests              # 更新套件
pip uninstall requests               # 移除套件
python -m pip install --upgrade pip  # 更新 pip 本身
```

> ⚠️ 一定要在 **Anaconda Navigator → Environment → 綠箭頭 → Open Terminal** 開的那個 terminal 裡執行。

### jieba — 中文斷詞

```python
import jieba

jieba.cut(sentence, cut_all=False)   # 精確模式（最常用）
jieba.cut(sentence, cut_all=True)    # 全模式，斷得更細
jieba.cut_for_search(sentence)       # 搜尋引擎模式
```

**自訂詞彙**（避免專有名詞被切斷）：

```python
jieba.add_word("即將結束", freq=None, tag=None)     # 單一詞

# 批次：寫成 dictionary.txt，每行格式「詞彙 權重 詞性」（空白分隔）
jieba.load_userdict("dictionary.txt")
```

> 權重越高，被斷成同一詞的機率越高；詞性對斷詞影響較小。

### PIL / Pillow — 圖片處理

```python
from PIL import Image

img = Image.open("cat.jpg")
img.size                              # (寬, 高) 像素
img.show()
img.resize((400, 200))
img.transpose(Image.ROTATE_180)
img.save("test.png")
```

### pytube — 下載 YouTube 影片

```python
from pytube import YouTube
YouTube(url).streams.get_highest_resolution().download()
```

> ⚠️ YouTube 一改版 `pytube` 就常壞，現在多改用 **`yt-dlp`**。

### qrcode — 產生 QR Code

```python
import qrcode
qr = qrcode.QRCode(box_size=10, border=4)   # box_size 大小、border 邊框粗細
qr.add_data("https://example.com")
img = qr.make_image(fill_color="black", back_color="white")
img.save("qr.png")
```

### pytesseract — OCR 圖片文字辨識

**兩步驟安裝**（缺一不可）：
1. 到官網下載並安裝 **Tesseract 執行檔**（Windows 64/32 位元，全程下一步），記下安裝路徑
2. `pip install pytesseract`

```python
import pytesseract
from PIL import Image

pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
print(pytesseract.image_to_string(Image.open("doc.png")))
```

## M23 自定義模組

把函數寫進獨立的 `.py` 檔（例如 `makefood.py`），就成了可重用的模組。

```python
import makefood
makefood.cook()                 # 要加模組名前綴

from makefood import cook       # 匯入後可直接用，不需前綴
from math import pi
```

## M24 執行緒與平行處理

**子執行序**：用 `subprocess.Popen()` 呼叫外部程式（如記事本），該程式以子執行序方式跑。

**平行化概念**：傳統程式逐行執行（序列化）；平行處理把工作拆給多核心 CPU 同時執行，最後合併結果。

```python
import threading

def worker():
    for i in range(5):
        print("child thread", i)

t = threading.Thread(target=worker)
t.start()          # 啟動
t.join()           # 等待結束

print("main thread")
```

執行結果會看到 **child thread 與 main thread 交錯輸出** —— 這就是平行的證據。

---

# Part 7 · 資料庫（M25–M27）

## M25 SQLite 介紹

SQLite 是 **Python 內建**的輕量級嵌入式資料庫，裝完 Python 就有，不用另外裝伺服器。

**資料庫四個名詞**：資料表 Table、記錄 Record/Row、欄位 Field/Column、資料值 Data Value。

**SQLite Browser**（圖形化檢視工具）：下載免安裝的 NoInstaller 版本 → 解壓縮 → 執行 `db-browser-for-sqlite.exe` → 「打開資料庫」→「Browse Data」看資料表與欄位值。

## M26 SQLite CRUD

SQL 是與資料庫溝通的語言；Python 只是透過 API 幫你把 SQL 送出去。

```python
import sqlite3

conn = sqlite3.connect("mydata.db")      # 檔案不存在會自動建立
cur = conn.cursor()

# CREATE 建表
cur.execute("""CREATE TABLE Student (
                   ID INTEGER, Name TEXT, Gender TEXT)""")

# INSERT 插入（用問號當佔位符，避免 SQL injection）
cur.execute("INSERT INTO Student VALUES (?, ?, ?)", (1, "Jason", "M"))
conn.commit()                            # ⚠️ 一定要 commit 才真的寫進去

# SELECT 查詢
cur.execute("SELECT * FROM Student WHERE Gender = ?", ("M",))
print(cur.fetchall())

# UPDATE 更新
cur.execute("UPDATE Student SET Name = ? WHERE ID = ?", ("Tom", 1))

# DELETE 刪除
cur.execute("DELETE FROM Student WHERE ID = ?", (1,))
conn.commit()
conn.close()                             # 操作完關閉連線
```

> 💡 也可省略 cursor 直接用 `conn.execute(...)`，較精簡。
> 💡 改完可在 SQLite Browser 按重新整理確認結果。

## M27 MySQL

MySQL 是功能更完整的關聯式資料庫，效能佳、成本低，商業應用廣泛。

**安裝**：官網下載 Windows 版（需 Oracle 帳號）；Python 端 `pip install mysqlclient`。

> 💡 **本訓練營不必手動裝** —— 本機已有 Docker `mysql`（`127.0.0.1:3306`，root / <你的密碼>），adminer 在 http://localhost:8080。
> 講義用的 `MySQLdb`（mysqlclient）在 Python 3 環境常裝不起來，實務改用 **`pymysql`** 或 `mysql-connector-python`，API 幾乎相同。

```python
import pymysql       # 講義寫 MySQLdb；pymysql 為現代等價寫法

conn = pymysql.connect(host="127.0.0.1", user="root",
                       passwd=os.environ["MYSQL_ROOT_PASSWORD"], db="myDatabase")
cur = conn.cursor()

cur.execute("SELECT VERSION()")
print(cur.fetchone())                   # ('8.0.22',)

# 建資料庫時先 DROP 再 CREATE，避免重複建立報錯
cur.execute("DROP DATABASE IF EXISTS myDatabase")
cur.execute("CREATE DATABASE myDatabase")

# 建表可指定欄位型別
cur.execute("""CREATE TABLE Employee (
                   Name VARCHAR(20), Age INT,
                   IsActive TINYINT(1), Salary FLOAT)""")

cur.execute("SELECT * FROM Employee")
for row in cur.fetchall():              # fetchall 取全部，搭配迴圈逐筆輸出
    print(row)

conn.commit()
conn.close()
```

CRUD 語法與 SQLite 相同：`INSERT INTO` / `SELECT * FROM` / `UPDATE ... SET` / `DELETE FROM`。

---

# Part 8 · Tkinter GUI（M28–M29）

Tkinter 是 Python **標準函式庫**內建的 GUI 工具，不用另外安裝就能做視窗程式。

## M28 基本元件

```python
from tkinter import *

win = Tk()                                    # 建立視窗物件
win.title("我的程式")

Label(win, text="姓名：").pack()               # 標籤
entry = Entry(win)                            # 單行文字方塊
entry.pack()

txt = Text(win, height=5, fg="blue")          # 多行文字區域，可設顏色字型
txt.insert(END, "預設文字")
txt.pack()

Button(win, text="送出", command=lambda: print(entry.get())).pack()   # 功能鈕

win.mainloop()                                # ⚠️ 一定要呼叫才會顯示視窗
```

> 元件建立後要呼叫 `.pack()`（或 `.grid()` / `.place()`）才會被放進視窗。

## M29 進階元件

| 元件 | 用途 | 重點 |
|---|---|---|
| `Scrollbar` | 捲軸 | 搭配 `Listbox`，用迴圈 `myList.insert(...)` 逐一塞選項 |
| `Radiobutton` | 選項鈕（單選） | 同組共用一個 variable |
| `Checkbutton` | 核取方塊（複選） | 各自一個 variable |
| `messagebox` | 對話方塊 | `showinfo`（資訊）/ `showwarning`（警告）/ `showerror`（錯誤） |
| `PhotoImage` | 顯示圖片 | 搭配 `Image.open()` 讀檔後載入 |
| `Menu` | 功能表 | `add_command()` 加 New/Open/Save/Close，支援多層選單 |

```python
from tkinter import messagebox
messagebox.showinfo("提示", "存檔成功")
messagebox.showwarning("警告", "檔案已存在")
messagebox.showerror("錯誤", "找不到檔案")
```

```python
menubar = Menu(win)
filemenu = Menu(menubar, tearoff=0)
filemenu.add_command(label="New",  command=new_file)
filemenu.add_command(label="Open", command=open_file)
menubar.add_cascade(label="File", menu=filemenu)
win.config(menu=menubar)
```

---

# 附錄 A · 全課踩坑總表

| # | 坑 | 正解 |
|---|---|---|
| 1 | 在系統 cmd 裡 `pip install`，Jupyter 卻找不到套件 | 一律用 Anaconda Navigator → Environment → Open Terminal（前綴會顯示環境名） |
| 2 | 裝了套件但 import 失敗 | Kernel → Restart |
| 3 | `In[*]` 一直不停 | 無窮迴圈 → Kernel → Restart |
| 4 | 混用 Tab 與空白 | `IndentationError`；統一 4 個空白 |
| 5 | `=` 與 `==` 搞混 | `=` 賦值、`==` 比較 |
| 6 | `input()` 拿來直接算數 | 一定是字串，要 `int()` / `float()` |
| 7 | `isidentifier()` 對 `"for"` 回 True | 要再加 `not keyword.iskeyword(name)` |
| 8 | `a.append(b)` 把整個 list 塞成一個元素 | 合併要用 `.extend()` |
| 9 | 改 tuple 元素 | `TypeError`；tuple 是 immutable |
| 10 | 巢狀迴圈印圖形，換行位置錯 | `print()` 要放在**外層**迴圈結尾 |
| 11 | 遞迴爆掉 | 預設上限 3000；先檢查 base case，再考慮 `sys.setrecursionlimit()` |
| 12 | 函數內改不到外部變數 | 要 `global` 宣告 |
| 13 | 多重繼承同名 method 行為不如預期 | 以**第一個**繼承的父類別為優先 |
| 14 | 子類別寫了 `__init__` 後父類別屬性不見 | 要 `super().__init__()` |
| 15 | 檔案用 `w` 模式把內容洗掉 | 要附加請用 `a` |
| 16 | 忘了 `f.close()` | 用 `with open(...) as f` |
| 17 | SQL 寫了但資料庫沒變 | 忘了 `conn.commit()` |
| 18 | Colab 閒置斷線資料不見 | 定期「檔案 → 下載 → 下載 .ipynb」 |
| 19 | Jupyter 切不到 D 槽 | Anaconda 與課程檔都放 C 槽，做完再搬 |
| 20 | 視窗程式跑起來沒反應 | 忘了 `win.mainloop()` |

# 附錄 B · 講義版本老舊處

| 講義寫法 | 現況 | 建議 |
|---|---|---|
| `pytube` | YouTube 改版後常壞 | 改用 `yt-dlp` |
| `MySQLdb` / `mysqlclient` | Python 3 下常裝不起來 | 改用 `pymysql` 或 `mysql-connector-python` |
| `"{}".format()` / `%` 格式化 | 仍可用 | 日常優先寫 **f-string** |
| Python 3.6 / 3.7 | 已 EOL | 本訓練營用 `ds`（**Python 3.13**） |
| Tesseract 需另裝引擎 | 仍是 | 或改用雲端 OCR / `rapidocr-onnxruntime`（中文較準，無需另裝引擎） |

---

## 延伸

- **語法查表** → `notes/MOC-python語法速查`（18 篇速查筆記）
- **習題解法** → `notes/drill-習題解法彙編`
- **課堂補充** → `notes/reference-課堂補充與踩坑`
- **下一步（把語法用到資料分析）** → `Data Science/Python数据分析与数据化运营/`
- **環境操作** → [`docs/environment.md`](../../docs/environment.md)（訓練營排程文件已於 2026-09-01 刪除）
