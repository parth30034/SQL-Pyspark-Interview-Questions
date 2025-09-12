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
