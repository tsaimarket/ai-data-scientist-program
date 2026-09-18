---
name: cheat-tkinter-gui
description: Tkinter 建立視窗、版面管理、按鈕/文字方塊/標籤/核取/選項/卷軸/對話方塊/功能表/Canvas 元件速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M28-29
---

# Tkinter GUI 速查

Tkinter＝Python 標準 GUI 函式庫（內建，免裝），可快速做桌面視窗程式。

## 最小骨架

```python
import tkinter as tk

win = tk.Tk()              # 建主視窗
win.title('我的程式')
win.geometry('300x200')    # 寬x高

tk.Label(win, text='Hello').pack()   # 放一個標籤
win.mainloop()             # 進入事件迴圈（必須，否則視窗一閃即逝）
```

## 版面管理（三選一，別混用）

| 管理員 | 方式 |
|---|---|
| `pack()` | 由上而下/側邊自動堆疊 |
| `grid(row, column)` | 表格定位 |
| `place(x, y)` | 絕對座標 |

## 常用元件

| 元件 | 用途 |
|---|---|
| `Label` | 顯示文字/圖 |
| `Button(text, command=fn)` | 按鈕（command 綁回呼函數） |
| `Entry` | 單行文字方塊 |
| `Text` | 多行文字區域 |
| `Checkbutton` | 核取方塊（多選） |
| `Radiobutton` | 選項紐（單選，靠 variable 分組） |
| `Scrollbar` | 卷軸 |
| `Canvas` | 畫圖形（線/矩形/橢圓/圖片） |
| `Menu` | 功能表列 |
| `messagebox` | 對話方塊（提示/確認/詢問） |

```python
def on_click():
    print(entry.get())          # 取 Entry 內容

entry = tk.Entry(win); entry.pack()
tk.Button(win, text='送出', command=on_click).pack()

from tkinter import messagebox
messagebox.showinfo('標題', '內容')      # 提示框
messagebox.askyesno('確認', '要刪除嗎?')  # 是非框
```

## 取/設值（變數綁定）

```python
var = tk.StringVar()
tk.Entry(win, textvariable=var)
var.get(); var.set('預設值')
```

延伸：按鈕 command 綁的是函數，見 [framework-函數](framework-函數.md)；事件處理常配例外，見 [cheat-例外處理與檔案io](cheat-例外處理與檔案io.md)。
