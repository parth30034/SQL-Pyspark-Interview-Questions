# Q001 — Campaign Spend Incremental Load

### 📖 Context
Marketing teams need daily aggregated spend per campaign.  
Source data comes in **incremental batches** (only new rows per day).  
We need to ensure only the **latest record per campaign per day** is used.

---

### ❓ Question
Write a SQL query to fetch the **latest spend per campaign per day**  
(based on `updated_at` timestamp).

---

### 📊 Input Dataset
```sql
CREATE TABLE campaign_spend (
    campaign_id INT,
    spend_date DATE,
    spend_amount DECIMAL(10,2),
    updated_at TIMESTAMP
);

INSERT INTO campaign_spend VALUES
(101, '2025-09-10', 500.00, '2025-09-10 08:00:00'),
(101, '2025-09-11', 600.00, '2025-09-11 08:00:00'),
(102, '2025-09-11', 400.00, '2025-09-11 09:00:00'),
(101, '2025-09-11', 650.00, '2025-09-11 12:00:00'); -- late arriving update
```

---

### ✅ Expected Output
| campaign_id | spend_date  | spend_amount |
|-------------|-------------|--------------|
| 101         | 2025-09-10  | 500.00       |
| 101         | 2025-09-11  | 650.00       |
| 102         | 2025-09-11  | 400.00       |

---

### 🗝️ SQL Solution
```sql
WITH ranked AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY campaign_id, spend_date ORDER BY updated_at DESC) AS rn
    FROM campaign_spend
)
SELECT campaign_id, spend_date, spend_amount
FROM ranked
WHERE rn = 1;
```
