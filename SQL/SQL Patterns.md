# SQL Interview Flashcards — 20 Patterns (Cheatsheet)

Quick, interview-ready flashcards. Each card has: **Pattern name**, **What it tests**, **Example interview prompt**, and a **canonical solution snippet** (ANSI/portable SQL). Use these for quick revision and whiteboard-style explanations.

---

## 1. Gaps & Islands
**Tests:** identifying consecutive ranges / streaks.
**Example:** Find user login streaks (start/end date) for consecutive days.
**Solution (islands by `date - row_number` trick):**
```sql
WITH t AS (
  SELECT user_id, login_date,
         ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) rn
  FROM logins
)
SELECT user_id,
       MIN(login_date) AS start_date,
       MAX(login_date) AS end_date
FROM (
  SELECT *, DATE_SUB(login_date, INTERVAL rn DAY) grp
  FROM t
) s
GROUP BY user_id, grp;
```

---

## 2. Top-N per Group
**Tests:** greatest-N-per-group (latest per customer / top k).
**Example:** Get latest order per customer.
**Solution:**
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) rn
  FROM orders
) t
WHERE rn = 1;
```

---

## 3. Running Totals / Moving Averages
**Tests:** window frames and cumulative logic.
**Example:** 7-day moving average of sales.
**Solution:**
```sql
SELECT dt, sales,
  AVG(sales) OVER (ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS ma_7
FROM daily_sales;
```

---

## 4. Percentiles / Median
**Tests:** ranking / percentile functions.
**Example:** Median order value.
**Solution:**
```sql
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY amount) AS median
FROM orders;
-- or NTILE for bucketing:
SELECT *, NTILE(4) OVER (ORDER BY amount) quartile FROM orders;
```

---

## 5. Recursive Hierarchies
**Tests:** tree traversal and recursive CTEs.
**Example:** Expand org chart from a manager.
**Solution:**
```sql
WITH RECURSIVE cte AS (
  SELECT id, manager_id, name FROM employees WHERE id = 1
  UNION ALL
  SELECT e.id, e.manager_id, e.name
  FROM employees e
  JOIN cte ON e.manager_id = cte.id
)
SELECT * FROM cte;
```

---

## 6. SCD / Change Detection (LEAD/LAG)
**Tests:** detecting state changes over time.
**Example:** Produce SCD Type-2 periods for salary history.
**Solution:**
```sql
SELECT id, salary, start_date,
       LEAD(start_date) OVER (PARTITION BY id ORDER BY start_date) AS next_start
FROM salary_history;
```

---

## 7. Sessionization / Time-based Grouping
**Tests:** break event stream into sessions.
**Example:** Group user events into sessions separated by >30 min.
**Solution:**
```sql
SELECT *, SUM(new_session) OVER (PARTITION BY user_id ORDER BY event_ts) session_id
FROM (
  SELECT *, CASE WHEN TIMESTAMPDIFF(MINUTE, LAG(event_ts) OVER (PARTITION BY user_id ORDER BY event_ts), event_ts) > 30 THEN 1 ELSE 0 END AS new_session
  FROM events
) t;
```

---

## 8. Self-Joins / Relational Division
**Tests:** "customers who bought all items" style queries.
**Example:** Find customers who bought all products in set (A,B,C).
**Solution:**
```sql
SELECT customer_id
FROM purchases
WHERE product IN ('A','B','C')
GROUP BY customer_id
HAVING COUNT(DISTINCT product) = 3;
```

---

## 9. Deduplication (Keep First/Last)
**Tests:** dedupe with window functions.
**Example:** Keep latest event per user.
**Solution:**
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY event_ts DESC) rn
  FROM events
) t WHERE rn = 1;
```

---

## 10. Set Operations & Anti-Joins
**Tests:** excluding sets or intersecting.
**Example:** Users who signed up but never purchased.
**Solution:**
```sql
SELECT s.user_id
FROM signups s
LEFT JOIN purchases p ON s.user_id = p.user_id
WHERE p.user_id IS NULL;
```

---

## 11. Ranking & Tie Handling
**Tests:** RANK vs DENSE_RANK implications.
**Example:** Top sales per region with ties preserved.
**Solution:**
```sql
SELECT * FROM (
  SELECT *, RANK() OVER (PARTITION BY region ORDER BY sales DESC) rnk
  FROM sales
) t WHERE rnk <= 3;
```

---

## 12. Pivot / Unpivot
**Tests:** reshaping data for reporting.
**Example:** Monthly sales per product as columns.
**Solution (conditional aggregation):**
```sql
SELECT product,
  SUM(CASE WHEN month = '2025-01' THEN sales ELSE 0 END) AS jan,
  SUM(CASE WHEN month = '2025-02' THEN sales ELSE 0 END) AS feb
FROM monthly_sales
GROUP BY product;
```

---

## 13. Window Function Tricks (Nth value, second max)
**Tests:** clever window usage without joins.
**Example:** Second highest salary per department.
**Solution:**
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) rn
  FROM employees
) t WHERE rn = 2;
```

---

## 14. Event Sequencing / Pattern Matching
**Tests:** order-sensitive detection.
**Example:** Detect users who did X then Y within 1 hour.
**Solution:**
```sql
SELECT user_id
FROM (
  SELECT user_id, event_type, event_ts,
         LEAD(event_type) OVER (PARTITION BY user_id ORDER BY event_ts) next_event,
         LEAD(event_ts) OVER (PARTITION BY user_id ORDER BY event_ts) next_ts
  FROM user_events
) t
WHERE event_type = 'X' AND next_event = 'Y' AND TIMESTAMPDIFF(MINUTE, event_ts, next_ts) <= 60;
```

---

## 15. Data Quality / Anomaly Checks
**Tests:** find duplicates, outliers.
**Example:** Find duplicate transaction_ids.
**Solution:**
```sql
SELECT transaction_id, COUNT(*) cnt
FROM transactions
GROUP BY transaction_id
HAVING COUNT(*) > 1;
```

---

## 16. Calendar / Date Filling (Generate series)
**Tests:** filling missing dates for time series.
**Example:** Fill dates with zero sales.
**Solution (using numbers/date dimension):**
```sql
-- assume calendar(date) exists
SELECT c.date, COALESCE(s.sales,0) sales
FROM calendar c
LEFT JOIN daily_sales s ON c.date = s.date
WHERE c.date BETWEEN '2025-01-01' AND '2025-01-31';
```

---

## 17. Cohort & Retention Analysis
**Tests:** cohort grouping + lagged activity.
**Example:** 7-day retention of users who signed up in week 1.
**Solution (cohort by signup_week):**
```sql
WITH cohorts AS (
  SELECT user_id, MIN(signup_date) signup_date, DATE_TRUNC('week', signup_date) cohort_week
  FROM users
  GROUP BY user_id
)
SELECT c.cohort_week, d.day_offset, COUNT(DISTINCT a.user_id) retained
FROM cohorts c
JOIN activity a ON a.user_id = c.user_id
CROSS JOIN UNNEST(GENERATE_ARRAY(0,30)) AS day_offset -- BigQuery syntax example
WHERE DATE_DIFF(a.activity_date, c.signup_date, DAY) = day_offset
GROUP BY c.cohort_week, day_offset;
```

---

## 18. Conditional Aggregates (Reporting)
**Tests:** multi-metric reporting in one pass.
**Example:** active vs churned users by month.
**Solution:**
```sql
SELECT month,
  SUM(CASE WHEN status = 'active' THEN 1 ELSE 0 END) active_cnt,
  SUM(CASE WHEN status = 'churned' THEN 1 ELSE 0 END) churned_cnt
FROM user_status
GROUP BY month;
```

---

## 19. As-of / Closest Match Joins
**Tests:** find nearest timestamp match (financial, event enrichment).
**Example:** Join price as-of trade time.
**Solution:**
```sql
SELECT t.*, p.price
FROM trades t
LEFT JOIN LATERAL (
  SELECT price FROM prices p
  WHERE p.asset = t.asset AND p.ts <= t.ts
  ORDER BY p.ts DESC LIMIT 1
) p ON TRUE;
```

---

## 20. Performance Rewrites (Avoid full scans)
**Tests:** understand indexes, partitions, rewrite with predicates.
**Example:** Rewrite query to use partition pruning and avoid UDF on predicate.
**Solution (guideline):**
- Push predicates on partitioned columns into WHERE.
- Avoid wrapping indexed column in functions.
- Replace `SELECT DISTINCT` + join with `EXISTS`/`IN` where appropriate.

---

### Quick tips for interview delivery
- Explain complexity and index/partition considerations.
- Show one correct solution and then an optimized alternative.
- Mention trade-offs (readability vs performance vs portability).

---



