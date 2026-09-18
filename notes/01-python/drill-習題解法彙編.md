---
name: drill-習題解法彙編
description: 《快速闖關 Python 語法世界》全課 Practice 習題的解題思路彙編（M3–M16），來源為課堂檢討錄音
metadata:
  type: drill
book: 快速闖關Python語法世界
chapter: M3-M16
---

# 習題解法彙編（Practice 檢討）

講義只給題目，**解法在課堂口頭檢討**——這篇補的就是那層。卡住時先看「思路」欄，真的想不出來再看寫法。

## M3 基礎語法

| 題 | 思路 |
|---|---|
| BMI 計算器 | 輸入身高（公尺）與體重 → `weight / (height ** 2)`；輸入要 `float()` |
| 圓面積 | 設 `Pi = 3.14`，`area = Pi * r * r`；輸入轉 `float` 才能算 |
| 攝氏轉華氏 | `f = c * 9/5 + 32`；輸入轉 `int` |

## M4–M7 運算元與資料型別

**六位數幸運數字**（前三位數字和 == 後三位數字和）—— 兩種做法，體會「數學解」vs「字串解」：

| 做法 | 思路 |
|---|---|
| 數學 | 用 `%` 取個位、`//` 去尾，逐位拆出六個數字 |
| 字串 | 把數字當字串切片，各位元 `int()` 後加總，再比前三後三 |

| 題 | 思路 |
|---|---|
| 海倫公式算三角形面積 | `S = (a+b+c)/2`；`area = (S*(S-a)*(S-b)*(S-c)) ** 0.5`。**`** 0.5` 就是開根號** |
| list 操作 | 切片取元素、`del lst[i]` 刪除 |
| 字典操作 | `d[key] = value` 直接新增 Key-Value Pair |
| 字串綜合題 | `"".join(list)` 黏接、`len()` 取長度、取前後兩字元再黏、`.replace()` 換重複字元（⚠️ 第一個字元要先留下再拼回）、取最後兩字元 `* 4` |

## M8 流程控制

| 題 | 思路 |
|---|---|
| 交換兩變數 A、B | 借助暫存 `temp = a; a = b; b = temp`（Python 也可 `a, b = b, a`） |
| 判斷母音 | `if ch in ['a','e','i','o','u']` —— 比串一長串 `or` 乾淨 |
| 分數轉等級 | 0–60 → C、60–90 → B、90–100 → A，用 if/elif/else |
| 判斷大寫/小寫/數字/特殊字元 | 用 `ord()` 取 ASCII 碼，做**區間比較** |

## M9 迴圈

| 題 | 思路 |
|---|---|
| 印金字塔 | 巢狀迴圈；第 i 層：空白 `level-i-1` 個、星號 `2i+1` 個 |
| 階乘 factorial | for 迴圈把 1 到 n 依序相乘 |
| 統計奇偶數個數 | `% 2 == 0` 判斷後各自累加 |
| 印 0–6 但跳過 2 和 6 | `continue` |
| 找 1500–2700 間同時被 7 和 5 整除 | 雙條件 `and` |
| 字串反轉 | `range` 反向迭代，或 `list.reverse()` + `join()` |
| 九九乘法表 | 巢狀迴圈 |
| 印右三角形 | 巢狀迴圈，注意換行 `print()` 放外層 |
| list 元素加總 | 手動累加（**理解原理優於直接用 `sum()`**） |
| 找最大數字 | 先假設第一個最大，逐一比較覆寫 |
| list 去重複 | 用 set 記錄已出現元素；或直接 `list(set(lst))` |
| 判斷質數 | `isPrime` 旗標 + 2 到 n/2 逐一試除，**找到整除者立刻 `break`** |
| 統計字元頻率 | 用 dictionary 累計 |
| 找最長單字 | 邏輯同「找最大數字」 |
| 計算向量距離 | 平方和後 `** 0.5` |
| 判斷迴文 | 比較原字串與反轉字串是否相同 |

## M10–M13 函數、lambda、遞迴

| 題 | 思路 |
|---|---|
| `multiply` 函數 | 基本 def + return |
| `factorial` 函數 | 迴圈版或遞迴版都寫一次，比較差異 |
| 大小寫字母計數 | 走訪字串 + `.isupper()` / `.islower()` |
| 1–20 平方 | for + list |
| 平均值函數 | `sum(lst) / len(lst)` |
| 組合公式 C(n, r) | `n! / (r! * (n-r)!)`，重用 `factorial` |
| 找子集合數 | 2^n 概念 |
| 兩 list 取交集 | 轉 set 用 `&`，或雙迴圈比對 |
| 連續三判斷 | 走訪時看相鄰三元素 |
| Blackjack 函數 | 條件分支練習 |
| 計算字母與數字個數 | `.isalpha()` / `.isdigit()` |
| 列印數字三角形 pattern | 巢狀迴圈 |

## M14–M15 物件導向

| 題 | 思路 |
|---|---|
| P1-1 `Circle` | 屬性 `radius`，method 算面積 `radius ** 2 * 3.14` |
| P1-2 `IOStream` | method 從鍵盤輸入字串、印出大寫版本 |
| P1-3 `Song` | `__init__` 傳入歌詞 list，`sing_me_a_song()` 逐行印出 |
| P2-1 `Vehicle` | 父類別衍生 `Car1`（red convertible / FER / 6萬）、`Car2`（blue van / Junk / 1萬），各自呼叫 `describe()` |
| P2-2 `Temperature` | 攝氏→華氏 `× 9/5 + 32`；華氏→攝氏 `(-32) × 5/9` |
| P2-3 `Time` | 時間相加**含進位處理**、顯示時分、換算總分鐘數 |
| P3 `Shape` 體系 | 父類別 `Shape`（`color`、`fill` 屬性 + 算面積 / get/set 顏色）；`Circle` 與 `Rectangle` 繼承，皆用 `super().__init__()` 再新增自己的屬性 |

## M16 檔案處理

| 題 | 思路 |
|---|---|
| 計算檔案字數 | 先把逗號 `.replace(",", " ")` → `.split()` → `len()` |
| 計算檔案行數 | `len(f.readlines())` |
| 找出最長英文單字 | 讀檔 → 對空白 split → 迴圈比較每個字長度 |

## 課堂交代的自主練習

- 把 `BankNet` 寫進 `__init__` 裡試試
- 在類別**外部**直接呼叫 `__calculateRate`，觀察會發生什麼（驗證封裝）
- 練 jieba 自訂詞典 `dictionary.txt`（格式：`詞彙 權重 詞性`，空白分隔）
- 跑多執行序範例，觀察 child thread 與 main thread **交錯執行**

---

延伸：語法查表見 [MOC-python語法速查](MOC-python語法速查.md)；線性教學版見同層 `Python完整教學_M1-M30.md`；課堂口述補充見 [reference-課堂補充與踩坑](reference-課堂補充與踩坑.md)。
