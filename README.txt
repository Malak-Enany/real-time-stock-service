**Real-Time Stock Price Service – Part 1 (Infrastructure Setup)**

---

## 1️⃣ هيكل المشروع

```
real-time-stock-service/
├─ docker-compose.yml
├─ services/
│   ├─ api/
│   │   ├─ Dockerfile
│   │   ├─ requirements.txt
│   │   └─ main.py
│   └─ worker/
│       ├─ Dockerfile
│       ├─ requirements.txt
│       └─ worker.py
```

---

## 2️⃣ محتوى الملفات الأساسية

### 2.1 docker-compose.yml

* يحتوي على إعدادات الخدمات: MySQL, Redis, Kafka, Zookeeper, API, Worker
* ينشئ الشبكة والحاويات تلقائيًا.

### 2.2 services/api/

* `Dockerfile` → لبناء صورة FastAPI
* `requirements.txt` → مكتبات Python: `fastapi, uvicorn, mysql-connector-python, redis, pydantic`
* `main.py` → placeholder API:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"status": "api placeholder running"}
```

### 2.3 services/worker/

* `Dockerfile` → لبناء صورة Worker
* `requirements.txt` → مكتبات Python: `kafka-python, mysql-connector-python, redis`
* `worker.py` → placeholder Worker مع طباعة الوقت:

```python
import time
from datetime import datetime

if __name__ == "__main__":
    while True:
        print(f"{datetime.now()} - worker placeholder running")
        time.sleep(5)
```

---

## 3️⃣ أوامر تشغيل المشروع

1. دخول فولدر المشروع:

```powershell
cd "D:\study\semester 7\IOT\real-time-stock-service"
```

2. تشغيل كل الخدمات وبناء الصور:

```powershell
docker compose up -d --build
```

3. التأكد من الحاويات شغالة:

```powershell
docker ps
```

---

## 4️⃣ اختبارات كل خدمة

### 4.1 FastAPI

* افتح المتصفح على:

```
http://localhost:8000/
```

* النتيجة المتوقعة:

```json
{"status":"api placeholder running"}
```

### 4.2 Worker

* متابعة الرسائل في الوقت الحقيقي:

```powershell
docker exec -it kafka_worker bash
>> python worker.py
```

* الرسائل المتوقعة كل 5 ثواني:

```
2025-12-02 20:40:12 - worker placeholder running
```

* للخروج من المتابعة: `Ctrl + C`

### 4.3 Redis

```powershell
docker exec -it real-time-stock-service-redis-1 redis-cli
PING
```

* النتيجة المتوقعة: `PONG`

### 4.4 MySQL

```powershell
docker exec -it stock_mysql mysql -u root -proot
SHOW DATABASES;
```

* القواعد المتوقعة:

```
information_schema
mysql
performance_schema
stocks_db
sys
```

---

## 5️⃣ دخول الحاويات للتجربة أو تعديل الكود

### Worker

```powershell
docker exec -it kafka_worker bash
python worker.py
```

### FastAPI

```powershell
docker exec -it fastapi_service bash
# تعديل main.py أو تجربة الأوامر داخل الحاوية
```

### الخروج من أي حاوية

```
exit
```

