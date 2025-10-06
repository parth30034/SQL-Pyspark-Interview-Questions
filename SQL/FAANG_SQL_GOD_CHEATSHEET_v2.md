
# 🧠 FAANG+ SQL GOD CHEATSHEET

🎯 **Goal:**  
Master every query archetype used in FAANG and high-bar interviews — from window logic to recursive reasoning to query optimization.


---

# 🚀 ADVANCED FAANG+ PATTERNS (21–30)

## **21. Dynamic Pivot (Unknown Columns)**
```sql
SELECT *
FROM crosstab(
  $$SELECT user_id, metric, value FROM user_metrics$$
) AS ct(user_id INT, clicks INT, impressions INT, revenue DECIMAL);
```

---

## **22. Bucketization / Histogram**
```sql
SELECT FLOOR(age / 10) * 10 AS age_bucket, COUNT(*) cnt
FROM users
GROUP BY 1
ORDER BY 1;
```

---

## **23. Complex Joins (Null Safe)**
```sql
SELECT *
FROM a
LEFT JOIN b
ON a.id = b.id
AND COALESCE(a.country, '') = COALESCE(b.country, '');
```

---

## **24. Query Plan / Index Reasoning**
```sql
-- Prefer range filters over function calls on indexed columns
WHERE order_ts >= '2025-10-06' AND order_ts < '2025-10-07';
```

---

## **25. Deduplication with Aggregate**
```sql
SELECT user_id, MAX(event_ts) AS latest_ts
FROM events
GROUP BY user_id;
```

---

## **26. String Aggregation**
```sql
SELECT order_id,
       STRING_AGG(product_name, ', ' ORDER BY product_name) AS products
FROM order_items
GROUP BY order_id;
```

---

## **27. Pivot with Multiple Measures**
```sql
SELECT region,
  SUM(CASE WHEN month='2025-01' THEN revenue ELSE 0 END) jan_revenue,
  COUNT(CASE WHEN month='2025-01' THEN order_id END) jan_orders
FROM orders
GROUP BY region;
```

---

## **28. Recursive Gap Filling**
```sql
WITH RECURSIVE dates AS (
  SELECT MIN(date_col) AS d FROM t
  UNION ALL
  SELECT DATE_ADD(d, INTERVAL 1 DAY)
  FROM dates
  WHERE d < (SELECT MAX(date_col) FROM t)
)
SELECT * FROM dates;
```

---

## **29. Top Percentile per Group**
```sql
SELECT *
FROM (
  SELECT *, PERCENT_RANK() OVER (PARTITION BY region ORDER BY spend DESC) AS p
  FROM customers
) t
WHERE p <= 0.10;
```

---

## **30. Recursive Path Flattening**
```sql
WITH RECURSIVE org AS (
  SELECT id, manager_id, name, name AS path
  FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.manager_id, e.name, CONCAT(o.path, ' > ', e.name)
  FROM employees e
  JOIN org o ON e.manager_id = o.id
)
SELECT * FROM org;
```

---

# 🧩 Bonus (FAANG++ Tier)

| Concept | Example |
|----------|----------|
| **ROLLUP / CUBE** | `GROUP BY ROLLUP(region, product)` |
| **GROUPING SETS** | `GROUP BY GROUPING SETS ((region), (product), ())` |
| **EXPLAIN Plans** | Show query optimizer understanding |
| **CTE Chains** | Multi-stage transformations |
| **Data Lineage Debugging** | Trace transformations via alias |

---

🧠 **Author’s Note:**  
This sheet is designed for senior data engineers and analytics professionals preparing for FAANG interviews or production-grade SQL challenges.
