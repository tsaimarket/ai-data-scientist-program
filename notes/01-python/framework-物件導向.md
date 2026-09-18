---
name: framework-物件導向
description: class 語法、self/attribute/method、__init__、封裝/繼承/多重繼承/多型 三大特徵速查
metadata:
  type: framework
book: 快速闖關Python語法世界
chapter: M14-15
---

# 物件導向 OOP 速查

Python 一切皆物件。OOP 三特徵：**封裝、繼承、多型**，目的＝提高重用性/擴充性/維護性。

## class 語法

```python
class Dog():                 # 類別名首字母大寫
    def __init__(self, name):   # 建構子，建立物件時自動執行
        self.name = name        # attribute（屬性）
    def bark(self):             # method（方法），第一參數必為 self
        print(f'{self.name} 汪汪')

d = Dog('小白')   # 創建物件（呼叫 __init__）
d.bark()          # 取用 method
d.name            # 取用 attribute
```

| 名詞 | 是什麼 | 取用 |
|---|---|---|
| attribute | class 內的變數 | `self.xxx` |
| method | class 內的函數 | 第一參數 `self` |
| `__init__` | 建構子，創建時自動跑 | 傳 self + 其他參數 |

## 1. 封裝 encapsulation

把 attribute/method **私有化**，外部不可直接存取 → 名稱前加 `__`：

```python
class Account():
    def __init__(self):
        self.__balance = 0       # 私有，外部 a.__balance 取不到
    def deposit(self, n):        # 對外提供 public 介面
        self.__balance += n
```

## 2. 繼承 inheritance

子類別（subclass）繼承父類別（superclass）的 attribute/method：

```python
class Animal():
    def eat(self): print('吃')

class Cat(Animal):       # Cat 繼承 Animal
    def meow(self): print('喵')

Cat().eat()   # 繼承來的
```
**多重繼承**：`class Son(Father, Uncle)` 可同時繼承多個父類別。

## 3. 多型 polymorphism

子類別定義與父類別**同名 method** → 覆寫（override）父類功能：

```python
class Animal():
    def sound(self): print('...')
class Dog(Animal):
    def sound(self): print('汪')    # 覆寫
class Cat(Animal):
    def sound(self): print('喵')    # 覆寫

for a in [Dog(), Cat()]:
    a.sound()    # 各自表現不同 → 多型
```

延伸：class 也是物件、mutable 概念見 [cheat-資料型別總覽](cheat-資料型別總覽.md)；把 class 存成模組重用見 [cheat-第三方與自定義模組](cheat-第三方與自定義模組.md)。
