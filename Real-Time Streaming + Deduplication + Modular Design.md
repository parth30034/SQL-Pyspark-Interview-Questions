✅ TOPIC-WISE Q&A SET
--------------------

### 🔗 Real-Time Streaming Joins & Aggregations

**1\. Can you join two streams in Structured Streaming? What are the limitations?****Answer:**Yes, PySpark allows **stream–stream joins** with **watermarking** on both sides.

**Requirements:**

*   Both streams must define a watermark
    
*   Join must have an **event-time window**
    

**Example:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df1.withWatermark("ts", "10 min").join(      df2.withWatermark("ts", "10 min"),      expr("df1.id = df2.id AND df1.ts BETWEEN df2.ts - interval 5 minutes AND df2.ts + interval 5 minutes")  )   `

**Limitation:** Increased state size → memory pressure.

**2\. How do you perform windowed aggregations in streaming pipelines?****Answer:**Use groupBy(window()) with watermarking.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   from pyspark.sql.functions import window, count  df.withWatermark("timestamp", "10 minutes") \    .groupBy(window("timestamp", "5 minutes")) \    .agg(count("*"))   `

**Use Case:** Count user logins every 5 minutes with tolerance for 10-minute delay.

### 🚀 Join Optimization for Large Volumes

**3\. What are your strategies to optimize a join between a 10GB and 1TB table in Spark?****Answer:**

*   Use **broadcast join** if the 10GB table fits in memory
    
*   Use **bucketing** (same number of buckets, same column)
    
*   Apply **partition pruning** using filter()
    
*   Use **Z-Ordering** in Delta for sorted reads
    

**Goal:** Minimize shuffle and memory pressure.

**4\. When would you choose bucketing over partitioning?****Answer:**

*   Use **bucketing** when join key is different from partition key
    
*   Bucketing helps **even data distribution** without full repartitioning
    

**Scenario:** You partition by event\_date but join by customer\_id.

### 🧱 Modular PySpark Architecture

**5\. How do you organize a large PySpark project into modular, reusable components?****Answer:**Typical structure:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   /project  │── configs/  │── transformations/  │── jobs/  │── utils/  └── main.py   `

*   Define reusable transformations in /transformations
    
*   Use config-driven job control
    
*   Make each step testable, importable, and isolated
    

**Bonus:** Add a logger in a utility module for consistent logging.

**6\. How do you make your PySpark code testable in isolation (unit testing)?****Answer:**

*   Keep transformations as **pure functions**
    
*   Use **mock DataFrames** (e.g., small samples via spark.createDataFrame)
    
*   Avoid hardcoded paths/configs
    
*   Use **pytest** or **unittest**
    

**Bonus:** Separate I/O logic from processing logic for better isolation.

### 🍬 Deduplication + Late Record Handling

**7\. How do you remove duplicates from a stream where records may arrive late?****Answer:**

*   Use ROW\_NUMBER() or last() over a **watermarked window**
    
*   Use stateful processing with a **deduplication key**
    

**Example:**Deduplicate on user\_id + event\_time with watermark of 10 minutes.

**8\. What’s the best way to deduplicate a huge batch dataset in Spark?****Answer:**Use window or dropDuplicates() on selected keys.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.withColumn(      "row_num",      row_number().over(Window.partitionBy("id").orderBy("updated_at desc"))  ).filter("row_num = 1")   `

**Avoid:** Full .distinct() on wide tables — expensive and slow.

### 🔁 Resilience and Audit

**9\. How do you handle data arriving multiple times in a pipeline due to retries or reprocessing?****Answer:**

*   Add **idempotency keys** like record\_id or hash\_id
    
*   Use dropDuplicates() before write
    
*   Use **MERGE** instead of append in Delta
    
*   Store input metadata (file name, timestamp) to detect replays
    

**10\. What metadata do you capture in your audit logs during a PySpark job?****Answer:**Typical audit table columns:

*   Job ID
    
*   Source Path
    
*   Start & End Time
    
*   Input record count
    
*   Output record count
    
*   Failed records (optional)
    

**Use Case:** Useful for debugging data loss, reprocessing, and SLA breach detection.