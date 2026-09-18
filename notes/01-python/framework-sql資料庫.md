---
name: framework-sql資料庫
description: SQLite（sqlite3）與 MySQL（pymysql/MySQLdb）的連線、建表、CRUD（增刪查改）SQL 語法速查
metadata:
  type: framework
book: 快速闖關Python語法世界
chapter: M25-27
---

# SQL 資料庫速查（SQLite / MySQL）

SQL＝跟關聯式資料庫溝通的語言（SQLite、MySQL、SQL Server 通用）。

## 名詞

| 詞 | 意義 |
|---|---|
| database | 資料庫 |
| table | 資料表（列＝record/row，欄＝field/column） |
| primary key | 主鍵（唯一識別） |

## SQLite（Python 內建，輕量、單檔）

工具：DB Browser for SQLite（sqlitebrowser.org）可視覺化看資料。

```python
import sqlite3
conn = sqlite3.connect('my.db')   # 無檔則自動建
cur = conn.cursor()

# 建表（先刪再建避免衝突）
cur.execute('DROP TABLE IF EXISTS users')
cur.execute('''CREATE TABLE users(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT, age INTEGER)''')

# 增（用 ? 參數化，防 SQL injection）
cur.execute('INSERT INTO users(name,age) VALUES(?,?)', ('Tom', 20))

# 查
cur.execute('SELECT * FROM users WHERE age > ?', (18,))
print(cur.fetchall())

# 改 / 刪
cur.execute('UPDATE users SET age=? WHERE name=?', (21, 'Tom'))
cur.execute('DELETE FROM users WHERE id=?', (1,))

conn.commit()    # 寫入變更（增刪改後必做）
conn.close()
```

## SQL CRUD 四句

| 動作 | 語法 |
|---|---|
| 增 | `INSERT INTO t(c1,c2) VALUES(v1,v2)` |
| 查 | `SELECT * FROM t WHERE 條件` |
| 改 | `UPDATE t SET c=v WHERE 條件` |
| 刪 | `DELETE FROM t WHERE 條件` |

## MySQL（關聯式 DB 主流，效能高、開源）

課程用 `MySQLdb`，**Python 3 改用 `pymysql` 或 `mysql-connector-python`**（API 幾乎相同）。

### 建庫、查版本、取值（課堂補充）

```python
# 建庫：先 DROP IF EXISTS 再 CREATE，避免重跑腳本時報「已存在」
cur.execute('DROP DATABASE IF EXISTS myDatabase')
cur.execute('CREATE DATABASE myDatabase')

cur.execute('SELECT VERSION()')
print(cur.fetchone())      # 取一筆 → ('8.0.22',)
```

| 取值方法 | 回傳 |
|---|---|
| `fetchone()` | **一筆** tuple（查單值/版本用） |
| `fetchall()` | **全部** list of tuple（配迴圈逐筆輸出） |
| `fetchmany(n)` | n 筆 |

MySQL 建表常用欄位型別（SQLite 只有 TEXT/INTEGER/REAL/BLOB，MySQL 要指定長度）：

| 型別 | 用途 |
|---|---|
| `VARCHAR(20)` | 變長字串，須給長度 |
| `INT` | 整數 |
| `TINYINT(1)` | 布林代用（0/1） |
| `FLOAT` / `DECIMAL(m,n)` | 浮點／金額（**金額用 DECIMAL,別用 FLOAT**） |

```python
import pymysql
conn = pymysql.connect(
    host='127.0.0.1', port=3306,
    user='root', password=os.environ['MYSQL_ROOT_PASSWORD'], database='test',
    charset='utf8mb4')
cur = conn.cursor()
cur.execute('SELECT * FROM users')
rows = cur.fetchall()
conn.commit(); conn.close()
```

## ⚠️ 對接訓練營（重要）

本機已有 Docker MySQL **`mysql`**（`127.0.0.1:3306`, root/<你的密碼>, 容器 `mysql-db-1`，adminer 在 :8080）。可直接拿上面 `pymysql` 範例連它練 L2 MySQL。env `ds` 已裝 `pymysql`。**絕對別 `docker compose down -v`**（會清空 DB）。細節見 [`docs/environment.md`](../../docs/environment.md)。

延伸：例外處理包住 DB 操作見 [cheat-例外處理與檔案io](cheat-例外處理與檔案io.md)。
