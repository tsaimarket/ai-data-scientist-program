---
name: cheat-第三方與自定義模組
description: pip 指令、jieba/Pillow/pytube/qrcode/tesseract 第三方模組、自定義模組 import / from import 速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M21-23
---

# 第三方模組與自定義模組速查

## pip 指令

第三方套件總站 pypi.org。

| 指令 | 作用 |
|---|---|
| `pip install <pkg>` | 安裝 |
| `pip install <pkg>==1.2.3` | 裝指定版本 |
| `pip install -U <pkg>` | 升級 |
| `pip uninstall <pkg>` | 移除 |
| `pip list` / `pip freeze` | 列已裝 / 輸出 requirements |
| `python -m pip install --upgrade pip` | 升級 pip 自己 |

## 課程介紹的第三方模組

| 模組 | 用途 | 現況註記 |
|---|---|---|
| `jieba` | 中文斷詞（精確/全/搜尋引擎模式，可加自訂詞） | 仍可用，NLP/質檢常用 |
| `Pillow` (PIL) | 影像處理（開圖/縮放/濾鏡/轉檔） | 標準影像庫 |
| `pytube` | 下載 YouTube 影片 | ⚠️ YT 改版常壞，改用 **yt-dlp** |
| `qrcode` | 產生 QR code | 可用 |
| `tesseract` (pytesseract) | OCR 圖片文字辨識 | 需另裝 Tesseract 引擎；中文用 rapidocr 更省事 |

```python
import jieba
jieba.lcut('我愛自然語言處理')   # ['我','愛','自然語言','處理']

from PIL import Image
img = Image.open('a.jpg'); img.resize((100,100)).save('b.jpg')

import qrcode
qrcode.make('https://example.com').save('qr.png')
```

### jieba 三模式與自訂詞典（課堂補充）

| 模式 | 呼叫 | 特性 |
|---|---|---|
| 精確模式 | `jieba.cut(s, cut_all=False)` | 預設，斷得最合理 |
| 全模式 | `jieba.cut(s, cut_all=True)` | 把所有可能的詞都切出來，**更細但會重疊** |
| 搜尋引擎模式 | `jieba.cut_for_search(s)` | 長詞再切短，適合建索引 |

避免特定詞被切斷（如產品名、遊戲術語）：

```python
jieba.add_word('即將結束', freq=None, tag=None)   # 單一詞
jieba.load_userdict('dictionary.txt')            # 批次
```

`dictionary.txt` 每行格式＝**`詞彙 空白 權重 空白 詞性`**：

| 欄 | 作用 |
|---|---|
| 權重 | **越高越不容易被切開**（唯一真正決定斷詞的欄） |
| 詞性 | 對斷詞結果**影響很小**，主要供後續詞性過濾用 |

→ 對接：HG 玩家聊天/評論違規偵測要先把遊戲術語灌進自訂詞典，否則「捕魚大師」會被切成「捕魚／大師」。配 ·。

### 其餘模組的實際呼叫

```python
# PIL 補充
img.size                              # (寬, 高) 像素
img.show()                            # 開系統看圖程式
img.transpose(Image.ROTATE_180)       # 旋轉/翻轉

# pytube（⚠️ 易壞，改用 yt-dlp）
from pytube import YouTube
YouTube(url).streams.get_highest_resolution().download()

# qrcode 帶參數
import qrcode
qr = qrcode.QRCode(box_size=10, border=4)   # box_size=每格像素、border=邊框格數
qr.add_data('https://example.com'); qr.make()
qr.make_image(fill_color='black', back_color='white').save('qr.png')

# pytesseract：⚠️ 裝完 pip 還要「在程式裡指定引擎路徑」才會動
import pytesseract
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
print(pytesseract.image_to_string(img))
```

## 自定義模組 import

把自己寫的 `.py` 當模組匯入別的程式重用。

```python
# mymod.py 內有 def hello(): ... 與變數 PI = 3.14

import mymod                 # 整包匯入
mymod.hello()                # 用 模組名.功能

import mymod as m            # 取別名
m.hello()

from mymod import hello, PI  # 只匯入指定功能（用時不加前綴）
hello()

from mymod import *          # 匯入全部（不建議，易命名衝突）
```

| 寫法 | 取用方式 |
|---|---|
| `import mod` | `mod.func()` |
| `import mod as m` | `m.func()` |
| `from mod import func` | `func()` |

延伸：模組內常放 class，見 [framework-物件導向](framework-物件導向.md)；安裝環境見 [concept-python執行與環境](concept-python執行與環境.md)。
