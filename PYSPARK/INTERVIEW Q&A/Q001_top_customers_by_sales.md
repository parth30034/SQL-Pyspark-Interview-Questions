# Q001 — Top Customers by Sales

### 📖 Context
An e-commerce company wants to find the **top 2 customers by total sales amount per month**.

---

### ❓ Question
Using PySpark, compute the **top 2 customers** for each month based on their purchase amount.

---

### 📊 Input Dataset
```python
data = [
    ("2025-01-15", "C001", 200),
    ("2025-01-20", "C002", 500),
    ("2025-01-25", "C001", 100),
    ("2025-02-05", "C003", 800),
    ("2025-02-15", "C002", 400),
    ("2025-02-25", "C001", 300),
]

columns = ["order_date", "customer_id", "amount"]
```

---

### ✅ Expected Output
| month   | customer_id | total_amount | rank |
|---------|-------------|--------------|------|
| 2025-01 | C002        | 500          | 1    |
| 2025-01 | C001        | 300          | 2    |
| 2025-02 | C003        | 800          | 1    |
| 2025-02 | C001        | 300          | 2    |

---

### 🔥 PySpark Solution
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import month, year, col, sum as F_sum
from pyspark.sql.window import Window
from pyspark.sql.functions import rank

spark = SparkSession.builder.getOrCreate()

df = spark.createDataFrame(data, columns)
df = df.withColumn("month", col("order_date").substr(1, 7))

monthly_sales = df.groupBy("month", "customer_id")                   .agg(F_sum("amount").alias("total_amount"))

windowSpec = Window.partitionBy("month").orderBy(col("total_amount").desc())
ranked = monthly_sales.withColumn("rank", rank().over(windowSpec))

result = ranked.filter(col("rank") <= 2)
result.show()
```
