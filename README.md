### ๐ซ์ฃผ์ : ํด๋น ์ ์ฅ์์ ๊ธฐ์ฌ๋ ๋ชจ๋  ์ ์ ์ ๋ณด ์ทจ๊ธ ์ฃผ์
------------------------------------
# ํ๋ก์ ํธ : prepro_flask_api_for_bi

## [ ์์ฝ ]  
- ๋ด์ฉ : DMS-BI ๋ฐ์ดํฐ ์ ์  ์์ค ์ฝ๋
- ํ๊ฒฝ : Python 3.7.2 - Flask, Blueprint(API)
- ๋ด๋น์ : ๊น์ฌ๋ฏผ
- ํ๋ก์ฐ : 
    1. API ํธ์ถ 
    2. Azure storage blob json ํ์ผ ํธ์ถ(Python azure client ์ฌ์ฉ)
    3. ๋ฐ์ดํฐํ๋ ์ ์ ์ฒ๋ฆฌ
    4. Azure SQL-Server ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ ์ฅ
    5. Microsoft Power BI ๋ฐ์ดํฐ ์๊ฐํ

------------------------------------
## [ git ๋ฐฐํฌ ๋ฐฉ๋ฒ ]
``` 
# deploy.sh

pkill -9 -ef flask                                    # ๊ธฐ์กด์ ์คํ์ค์ด๋ Flask ํ๋ก์ธ์ค kill
deactivate                                            # ๊ธฐ์กด์ ํ์ฑํ ํ๋ Python ๊ฐ์ํ๊ฒฝ ์ข๋ฃ
rm -rf /home/bi_data_flask                            # ๊ธฐ์กด์ ๋ฐฐํฌ ๋์ด ์๋ bi_data_flask ๋ ํฌ์งํ ๋ฆฌ ์ญ์ 
cd /home                                              # /home ๊ฒฝ๋ก ์ด๋

# Azure DevOps ANNA-Project ํ๋ก์ ํธ์ bi_data_flask ๋ ํฌ์งํ ๋ฆฌ git clone
git clone https://-----

source /home/envs/.bi_data_flask/bin/activate         # Python ๊ฐ์ํ๊ฒฝ ํ์ฑํ
pip install -r /home/bi_data_flask/requirements.txt   # Python ๊ฐ์ํ๊ฒฝ์ ๋ชจ๋ ์ค์น
export FLASK_APP=/home/bi_data_flask/run.py           # FLASK_APP ํ๊ฒฝ๋ณ์์ run.py ํ์ผ ๊ฒฝ๋ก ๋ฑ๋ก
nohup flask run -h 0.0.0.0                            # nohup์ ์ด์ฉํ์ฌ Background ์์ Host๊ฐ 0.0.0.0์ธ ์ํ๋ก Flask ์๋ฒ ์คํ
```
------------------------------------
## [ Cron ์ค์ผ์ค ]
- ์์ฝ :
  - ๋ด์ฉ : ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ์ฅ๋์ด ์๋ ๊ฐ ์ฌ์ดํธ์ ์๋น์ค ์ ๋ณด๋ฅผ ํธ์ถํ์ฌ ์๋์ ๊ฐ์ด Crontab์ ๋ฑ๋ก ๋์ด ์๋ ๋ช๋ น์ ๋ฐ๋ผ ์ ์  API ์คํ
  - ๋ฐ์ดํฐ๋ฒ ์ด์ค ์์น : /bi_data_flask/batch/bi_data_flask.db
  - ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ข๋ฅ : SQLite3
```
# 1์๊ฐ ๋ง๋ค ์คํํ๋ ์๋น์ค ์ฑ(๋งค์๊ฐ 05๋ถ ์คํ)
05 */1 * * *    sudo docker exec anna_flask /home/envs/.anna-data-flask/bin/python3 /home/bi_data_flask/batch/batch_run.py 1  >> /home/devel/logs/batch_run_1_hour.log 2>&1

# 24์๊ฐ ๋ง๋ค ์คํํ๋ ์๋น์ค ์ฑ(๋งค์ผ ์ค์  01์ 05๋ถ ์คํ)
05 01 * * *    sudo docker exec anna_flask /home/envs/.anna-data-flask/bin/python3 /home/bi_data_flask/batch/batch_run.py 24  >> /home/devel/logs/batch_run_24_hour.log 2>&1
``` 
------------------------------------
## [ ์์ค์ฝ๋ ํธ์ง ๋ฐ git ๊ด๋ฆฌ ๋ฐฉ๋ฒ ]
1. ์์ํ  ์์ญ์์ git clone(Linux ํ๊ฒฝ์์ ์คํ ๊ถ์ฅ)
```
$ git clone https://------
```
2. ํ์ด์ฌ ๋ชจ๋ ๊ด๋ฆฌ์ pip ์๊ทธ๋ ์ด๋
```
$ python3 -m pip install --upgrade pip
```
3. ํ์ด์ฌ ๊ฐ์ํ๊ฒฝ ๋ชจ๋ ์ค์น
```
$ pip install virtualenv
```
4. ํ์ด์ฌ ๊ฐ์ํ๊ฒฝ ์์ฑ
```
$ virtualenv [๊ฐ์ํ๊ฒฝ ์ด๋ฆ]
์) virtualenv .envs
```
5. ํ์ด์ฌ ๊ฐ์ํ๊ฒฝ ํ์ฑํ
```
$ source .envs/bin/activate
```
6. bi_data_flask ์คํ ํ์ ๋ชจ๋ ์ค์น
```
$ pip install -r bi_data_flask/requirements.txt
```
7. ์์ค์ฝ๋ ํธ์ง
```
# ์ฝ๋ ํธ์ง
```
8. git ํ์คํ ๋ฆฌ ์ ์ฅ
```
$ git add --all                             # ํ์ฌ ์ดํ์ ๋ชจ๋  ํ์ผ, ๋๋ ํ ๋ฆฌ ๋ฑ ์ ์ฅ ์ ์ธ
$ git add .                                 # ํ์ฌ ์ดํ์ ๋ชจ๋  ํ์ผ, ๋๋ ํ ๋ฆฌ ๋ฑ ์ ์ฅ ์ ์ธ
$ git commit -m "first commit message"      # ์ ์ฅ ์ ์ธํ ๋ชจ๋  ํ์คํ ๋ฆฌ Commit ๊ณผ Commit ๋ฉ์์ง ๋ฑ๋ก
$ git pull origin main                      # git ์ ์ฅ์์์ ํ์คํ ๋ฆฌ PULL ๋ค์ด ๋ฐ๊ธฐ(๋ณ๊ฒฝ ์ฌํญ์ ๋ํด ์๋ Merge ์๋ํจ)
$ git push origin main                      # git ์ ์ฅ์์ ํ์คํ ๋ฆฌ PUSH ๋ฐ์ด ๋ฃ๊ธฐ
```
------------------------------------
## [ ์๋ฃ ๊ตฌ์กฐ ]
- ์๋ฃ๊ตฌ์กฐ ์๋ฐ์ดํธ ์ tree ์๋ฃ ํจ๊ป ์๋ฐ์ดํธ 
```
anna-data-flask/
.
|-- README.md
|-- __pycache__
|   `-- run.cpython-37.pyc
|-- api
|   |-- v1
|   |   |-- __init__.py
|   |   |-- v1_helps.py
|   |   |-- v1_pre_aruba.py
|   |   |-- v1_pre_divas.py
|   |   |-- v1_pre_videotransfer.py
|   |   |-- v1_pre_vms.py
|   |   |-- v1_pre_vurixdms.py
|   |   `-- v1_sql.py
|   `-- v2
|       |-- __init__.py
|       |-- __pycache__
|       |   |-- __init__.cpython-37.pyc
|       |   |-- v2_helps.cpython-37.pyc
|       |   |-- v2_pre_aruba.cpython-37.pyc
|       |   |-- v2_pre_common.cpython-37.pyc
|       |   |-- v2_pre_divas.cpython-37.pyc
|       |   |-- v2_pre_forest.cpython-37.pyc
|       |   |-- v2_pre_videotransfer.cpython-37.pyc
|       |   |-- v2_pre_vms.cpython-37.pyc
|       |   |-- v2_pre_vurixdms.cpython-37.pyc
|       |   |-- v2_pre_weather.cpython-37.pyc
|       |   `-- v2_sql.cpython-37.pyc
|       |-- v2_helps.py
|       |-- v2_pre_aruba.py
|       |-- v2_pre_common.py
|       |-- v2_pre_divas.py
|       |-- v2_pre_forest.py
|       |-- v2_pre_videotransfer.py
|       |-- v2_pre_vms.py
|       |-- v2_pre_vurixdms.py
|       |-- v2_pre_weather.py
|       `-- v2_sql.py
|-- batch
|   |-- batch_run.py
|   |-- bi_data_flask.db
|   |-- db_query.py
|   `-- old_db
|       |-- bi_data_flask(20210901).db
|       `-- bi_data_flask(20210923).db
|-- requirements.txt
|-- research
|   |-- azure
|   |   `-- blob_client_test.py
|   `-- common
|       `-- replace_test_01.py
`-- run.py
``` 
------------------------------------
## [ API ์ฌ์ฉ๋ฐฉ๋ฒ ]
### 1. Python
```
import requests
import json

url = "http://[ํธ์คํธ]:5000/api/v2/data/vms/vms_device_model"

payload = json.dumps({
  "conn_str": [์คํ ๋ฆฌ์ง์ฐ๊ฒฐ๋ฌธ์์ด],
  "container": [์ปจํ์ด๋์ด๋ฆ],
  "service": [์๋น์ค์ด๋ฆ],
  "table": [ํ์ด๋ธ์ด๋ฆ],
  "target": [
    [BLOB JSON ์ด๋ฆ],
    [BLOB JSON ์ด๋ฆ]...
  ],
  "start_time": [yyyy-MM-dd HH:mm:ss],
  "end_time": [yyyy-MM-dd HH:mm:ss]
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

---------------------------------------------------------------------------
import requests
import json

url = "http://127.0.0.1:5000/api/v2/data/vms/vms_device_model"

payload = json.dumps({
  "conn_str": "XXXXX",
  "container": "dmsbi",
  "service": "vms",
  "table": "vms_device_model",
  "target": [
    "device_model.json"
  ],
  "start_time": "2021-08-24 12:00:00",
  "end_time": "2021-08-24 12:00:00"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
``` 

### 2. cURL
```
curl --location --request POST 'http://127.0.0.1:5000/api/v2/data/vms/vms_device_model' \
--header 'Content-Type: application/json' \
--data-raw '{
    "conn_str":"XXXXX",
    "container":"dmsbi",
    "service":"vms",
    "table":"vms_device_model",
    "target":["device_model.json"],
    "start_time":"2021-08-24 03:00:00",
    "end_time":"2021-08-24 12:00:00"
}'
``` 

------------------------------------
## [ ์ฐธ๊ณ  ์๋ฃ ]
- git ์์ค์ฝ๋ ๋ณด์ : https://github.com/sobolevn/git-secret
