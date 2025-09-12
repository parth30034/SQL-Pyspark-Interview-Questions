# 💡 Interview Q&A Contribution Guide

This folder is for **real-world Data Engineering interview questions**.  
We focus on **SQL, PySpark, Python, and System Design** questions that are practical, scenario-based, and relevant for DE interviews.

---

## 📌 Guidelines for Adding Questions

1. **Choose the correct folder**:
   - `sql/interview-questions/` → SQL-specific interview questions  
   - `pyspark/interview-questions/` → PySpark interview questions  
   - `python/interview-questions/` → Python / coding questions relevant to DE  
   - `system-design/interview-questions/` → System design & architecture for DE  

2. **File naming convention**:
   - Use the format:  
     ```
     Q<3-digit-number>_<short-title>.md
     ```
   - Example:  
     ```
     Q002_window_functions.md
     Q010_kafka_streaming_design.md
     ```

3. **Question File Template** (copy & use this for each new question):

```markdown
# QXXX — <Short Descriptive Title>

### 📖 Context
Provide a short description of the scenario.  
For example:  
*A data engineering team receives daily logs from multiple servers. They need to find the top 3 error codes per day.*

---

### ❓ Question
Write a SQL/PySpark query to solve the above problem.  
(Add any constraints, assumptions, or data model details here.)

---

### 📊 Input Dataset (Sample)
```sql
CREATE TABLE server_logs (
    event_time TIMESTAMP,
    server_id  INT,
    error_code STRING
);

INSERT INTO server_logs VALUES
('2025-09-10 08:00:00', 1, '500'),
('2025-09-10 09:00:00', 2, '404'),
('2025-09-10 10:00:00', 1, '500'),
('2025-09-10 11:00:00', 3, '403'),
('2025-09-11 08:30:00', 1, '500'),
('2025-09-11 09:15:00', 2, '500'),
('2025-09-11 10:45:00', 2, '404');
```

---

### ✅ Expected Output
| event_date  | error_code | error_count | rank |
|-------------|------------|-------------|------|
| 2025-09-10  | 500        | 2           | 1    |
| 2025-09-10  | 404        | 1           | 2    |
| 2025-09-10  | 403        | 1           | 2    |
| 2025-09-11  | 500        | 2           | 1    |
| 2025-09-11  | 404        | 1           | 2    |

---

### 💡 Hint
Use `DATE(event_time)` for grouping and a **window function** with `RANK()`.

---

### 🗝️ Example SQL Solution
```sql
WITH daily_errors AS (
    SELECT 
        DATE(event_time) AS event_date,
        error_code,
        COUNT(*) AS error_count
    FROM server_logs
    GROUP BY DATE(event_time), error_code
)
SELECT event_date, error_code, error_count,
       RANK() OVER (PARTITION BY event_date ORDER BY error_count DESC) AS rank
FROM daily_errors
WHERE rank <= 3;
```

---

### 🔥 Example PySpark Solution
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, count as F_count, to_date
from pyspark.sql.window import Window
from pyspark.sql.functions import rank

spark = SparkSession.builder.getOrCreate()

data = [
    ("2025-09-10 08:00:00", 1, "500"),
    ("2025-09-10 09:00:00", 2, "404"),
    ("2025-09-10 10:00:00", 1, "500"),
    ("2025-09-10 11:00:00", 3, "403"),
    ("2025-09-11 08:30:00", 1, "500"),
    ("2025-09-11 09:15:00", 2, "500"),
    ("2025-09-11 10:45:00", 2, "404"),
]

cols = ["event_time", "server_id", "error_code"]

df = spark.createDataFrame(data, cols)
df = df.withColumn("event_date", to_date("event_time"))

daily_errors = df.groupBy("event_date", "error_code").agg(
    F_count("*").alias("error_count")
)

windowSpec = Window.partitionBy("event_date").orderBy(col("error_count").desc())
ranked = daily_errors.withColumn("rank", rank().over(windowSpec))

result = ranked.filter(col("rank") <= 3)
result.show()
```

---

## 🌟 Tips for Contributors
- Always **provide both SQL and PySpark versions** if possible.  
- Keep datasets **small but realistic** (3–10 rows is enough for illustration).  
- Use the provided template to ensure **consistency across all questions**.  
- Add your file under the right subject → `interview-questions/`.  
- Update the **main README index** after adding a new question.  
