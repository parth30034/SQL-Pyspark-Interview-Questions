✅ TOPIC-WISE Q&A SET
--------------------

### 🟧 Advanced Delta Lake Operations

**1\. What is a Delta MERGE and how does it help with Change Data Capture (CDC)?****Answer:**MERGE in Delta allows **UPSERT** logic (update existing, insert new).

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   target.alias("t").merge(      source.alias("s"), "t.id = s.id"  ).whenMatchedUpdateAll().whenNotMatchedInsertAll().execute()   `

**Use Case:** Ingest CDC logs from Kafka or a landing zone without duplicates.

**2\. What is schema evolution in Delta and how do you enable it?****Answer:**Schema evolution allows new columns to be added during write.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.write.option("mergeSchema", "true").format("delta").mode("append").save(path)   `

**Risk:** Can lead to inconsistent schema if not versioned; handle with governance.

**3\. What’s the difference between overwrite and merge in Delta Lake? When to use each?****Answer:**

*   overwrite: Replaces data — risky if partition is missed
    
*   merge: Smart conditional logic (insert/update/delete)
    

**Use Case:**Use overwrite only for batch rewrites; use merge for daily incremental loads or updates.

### 📶 Structured Streaming in PySpark

**4\. How does PySpark Structured Streaming differ from batch processing?****Answer:**

*   Streams are **unbounded**, processed in **micro-batches**
    
*   Requires **checkpointing** and **state management**
    
*   Uses writeStream() instead of write()
    

**Use Case:** Kafka-to-Delta pipelines for real-time data ingestion.

**5\. What is watermarking in streaming and why is it important?****Answer:**Watermarking allows handling **late data** while managing **state size**.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   withWatermark("event_time", "10 minutes")   `

**Use Case:**If a record comes 5 minutes late, it will still be processed. Beyond 10 minutes — dropped.

**6\. What’s a checkpoint in streaming? What happens if you don’t use one?****Answer:**

*   Checkpoint stores progress (offsets, state).
    
*   Without it, on restart, the streaming job **restarts from the beginning**.
    

**Required For:** Stateful operations, joins, aggregations in streams.**Storage Tip:** Use persistent storage (e.g., DBFS, S3) for durability.

### 🔍 Debugging + Resilience in Production

**7\. Your Spark job keeps failing at the same stage. What do you investigate?****Answer:**

*   Check Spark UI → failed stage → skewed partition or null joins
    
*   Review executor memory (OOM?)
    
*   Logs for OutOfMemoryError, TaskNotSerializable
    

**Pro Tip:** Reduce shuffle size or use broadcast() if applicable.

**8\. You see java.lang.OutOfMemoryError: GC Overhead Limit Exceeded. What does this mean and how to resolve it?****Answer:**It means too much time is spent on **garbage collection**.

**Fixes:**

*   Reduce partition size
    
*   Avoid wide transformations before filters
    
*   Increase executor memory
    
*   Break down processing into multiple steps
    

**9\. How do you test a PySpark pipeline locally before deploying to cloud?****Answer:**

*   Use **local mode** Spark (master="local\[\*\]")
    
*   Sample datasets (e.g., 1000 rows)
    
*   Use mocks/stubs for external dependencies
    
*   Validate transformation logic with unit tests
    

**Bonus:** Create test utilities to reuse across jobs.

### 🧾 Lineage + Audit + Monitoring

**10\. How do you track which files were processed in a PySpark ingestion job?****Answer:**

*   Log all input file paths:
    

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.inputFiles()   `

*   Store metadata (file name, row count, status, timestamp) in an audit table.
    

**Tip:** For streaming, use **checkpoint + input watermark + file triggers** for traceability.

✅ **Summary:**This module focuses on **resilient pipeline design**, **MERGE strategies**, **streaming ingestion**, and **debugging Spark production jobs** — ideal for **data engineer interviews** testing ownership and reliability skills.