# 開發環境（conda ＋ VSCode ＋ Docker MySQL）

本學程的課程文件預設用瀏覽器版 Jupyter；這裡記錄的是我實際採用的替代組合：
**conda 環境 ＋ VSCode notebook ＋ Docker 版 MySQL**。

## 一、Python 環境

建一個獨立的 conda 環境（以下用 `ds` 當名字）：

```bash
conda create -n ds python=3.11
conda activate ds
pip install jupyter notebook numpy pandas matplotlib scikit-learn             selenium beautifulsoup4 requests pymysql
```

深度學習（專案 6–9 才需要）：

```bash
pip install tensorflow keras torch torchvision
```

> ⚠️ Windows 原生的 TensorFlow 只支援 CPU。學習用足夠，但訓練 CNN 會明顯偏慢；
> 要加速可把那幾個專案改到 Google Colab 的免費 GPU 上跑。

## 二、用 VSCode 取代瀏覽器版 Jupyter

需要擴充套件：**Python**（`ms-python.python`）＋ **Jupyter**（`ms-toolsai.jupyter`）。

**跑 `.ipynb`**
1. VSCode 開啟 `.ipynb`
2. 右上角 **Select Kernel → Python Environments → `ds`**
3. 每個 cell 按 `Shift+Enter`

**跑 `.py`**
1. 開 `.py`，右下角點 Python 版本選 `ds` 當 interpreter
2. 右上 ▶ 執行；或終端機 `conda activate ds` 後 `python xxx.py`
3. 小技巧：`.py` 裡用 `# %%` 分隔，就能像 notebook 一樣一段段跑（Interactive Window）

**新建筆記本**：`Ctrl+Shift+P` → `Jupyter: Create New Blank Notebook` → 選 `ds` kernel

## 三、MySQL（Docker）

用 [`docker-compose.example.yml`](docker-compose.example.yml)。複製成 `docker-compose.yml`，
並建立 `.env`：

```
MYSQL_ROOT_PASSWORD=<自己設一組密碼>
```

啟動：

```bash
docker compose up -d
```

| 項目 | 值 |
|---|---|
| MySQL 版本 | 5.7.33 |
| 主機 / Port | `127.0.0.1` / `3306` |
| 帳號 | `root` |
| 密碼 | 由 `.env` 的 `MYSQL_ROOT_PASSWORD` 決定 |
| adminer 網頁介面 | http://localhost:8080/ |

**adminer 登入**：System = `MySQL`、Server = **`db`**（不是 localhost）、Username = `root`。

**Python 連線**

```python
import os, pymysql

conn = pymysql.connect(
    host="127.0.0.1", port=3306,
    user="root", password=os.environ["MYSQL_ROOT_PASSWORD"],
    database="your_db", charset="utf8mb4",
)
cur = conn.cursor()
cur.execute("SELECT VERSION()")
print(cur.fetchone())
conn.close()
```

**容器操作**

- 看狀態：`docker ps`
- 停 / 起：`docker compose stop` / `docker compose start`
- ⚠️ `docker compose down -v` 會**連 volume 一起刪掉 → 整個資料庫清空**。平常別加 `-v`。

## 四、常見狀況

| 症狀 | 原因 |
|---|---|
| VSCode 找不到 kernel | 重開 VSCode，或 `Ctrl+Shift+P` → `Python: Select Interpreter` 先選一次 |
| pymysql `Connection refused` | Docker Desktop 沒開，或容器沒起（`docker ps` 確認） |
| import 某套件失敗 | kernel／interpreter 選到 base 或系統 Python，不是 `ds` |
| 要裝新套件 | `conda activate ds` 後 `pip install <套件>` |
