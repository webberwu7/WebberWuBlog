---
title: "Python - Bigquery 筆記"
date: 2022-08-18T00:05:39+08:00
tags: ['python', 'bigquery']
---

# 需要套件
1. google-cloud-bigquery => 串接bigquery的套件
2. pnadas => 抓取後製作成dataframe的套件
3. db-dtype => 轉換成dataframe的套件

### pip3 install
```bash
pip3 install google-cloud-bigquery
pip3 install pandas
pip3 install db-dtypes
```

### requirements.txt
```bash
pip install -r requirements.txt
```
```txt
# requirements.txt
google-cloud-bigquery==3.3.1
pandas==1.4.3
db-dtypes==1.0.3
```

# 簡單使用方法
```python3
# 設定讀取 google credentials 的路徑
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "config/Crawler-Bigquery.json"

# 啟動 bigquery client
client = bigquery.Client()

# 寫下你要 query 的語法
QUERY = (
    """
    YOUR QUERY
    """
)
query_job = client.query(QUERY)
response = query_job.to_dataframe()

print(response)
```
