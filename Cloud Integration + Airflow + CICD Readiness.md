✅ TOPIC-WISE Q&A SET
--------------------

### ☁️ Integrating PySpark with External Systems

**1\. How do you read and write data to S3 using PySpark in Databricks or EMR?**
**Answer:** Use spark.read and write with S3 path.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df = spark.read.csv("s3a://bucket-name/folder/")  df.write.format("parquet").mode("overwrite").save("s3a://bucket/output/")   `

**Credentials:** Set AWS keys in environment or use IAM roles.**Tip:** Avoid small files → use coalesce() or OPTIMIZE for Delta.

**2\. How do you connect PySpark to Snowflake?****Answer:**Use **Snowflake Spark Connector**.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sfOptions = {    "sfURL": ".snowflakecomputing.com",    "sfDatabase": "DEMO_DB",    "sfSchema": "PUBLIC",    "sfUser": "USERNAME",    "sfPassword": "PASSWORD",    "sfWarehouse": "COMPUTE_WH"  }  df = spark.read \    .format("snowflake") \    .options(**sfOptions) \    .option("dbtable", "MY_TABLE") \    .load()   `

**Best Practice:** Use secrets from key vaults or environment variables.

**3\. What are best practices for reading data from Kafka into a PySpark streaming job?**
**Answer:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df = spark.readStream \    .format("kafka") \    .option("kafka.bootstrap.servers", "broker:9092") \    .option("subscribe", "topic-name") \    .load()   `

**Important Settings:**

*   startingOffsets: latest or earliest
    
*   failOnDataLoss: false (in prod)
    
*   Handle binary to string conversion for key/value.
    

### 🧭 Workflow Scheduling & Orchestration

**4\. How do you trigger PySpark jobs via Airflow?****Answer:**Use SparkSubmitOperator or a custom PythonOperator.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   from airflow.providers.apache.spark.operators.spark_submit import SparkSubmitOperator  task = SparkSubmitOperator(      application="/path/to/job.py",      conn_id="spark_conn",      task_id="run_spark_job",      dag=dag  )   `

**Pro Tip:** Pass dynamic parameters via Airflow Variable or dag\_run.conf.

**5\. How do you schedule a daily PySpark job using Databricks Workflows?**
**Answer:**

*   Create a Job from notebook or Python script
    
*   Add a cluster policy and schedule (e.g., daily at 7 am)
    
*   Configure email alerts
    

**Parameters:** Use widgets or dbutils.widgets.get() for dynamic inputs.

### 🧰 Recovery, Monitoring & Alerting

**6\. How do you recover a failed Spark job that processed half the data?**
**Answer:**

*   Track processed files or checkpoint states
    
*   Use an audit table for metadata
    
*   Resume from failed partition/file
    
*   Make pipeline **idempotent** (using deduplication keys or MERGE)
    

**7\. What monitoring tools do you use to observe Spark jobs in production?**
**Answer:**

*   **Spark UI** (stages, DAG, task failures)
    
*   **Databricks Job Runs** dashboard
    
*   **Prometheus + Grafana** for metrics
    
*   **Airflow task logs** for workflow debugging
    

**Important Metrics:** Job duration, shuffle size, skew, memory usage.

**8\. What causes a Spark executor to fail and how do you fix it?**
**Common Reasons:**

*   Out of memory
    
*   Task skew
    
*   Long GC pause
    

**Fixes:**

*   Repartition or broadcast() small tables
    
*   Increase executor memory
    
*   Avoid wide transformations
    
*   Inspect Spark UI logs.
    

### 🧱 Deployment & CI/CD Readiness

**9\. How do you manage deployment of PySpark code in multi-env (dev, qa, prod)?**
**Answer:**

*   Use config-driven code (YAML/JSON)
    
*   Store env-specific secrets separately
    
*   Use CI/CD pipelines (GitHub Actions, Azure DevOps, Jenkins)
    
*   Version code in Git + automate job triggers
    

**10\. What are your steps before promoting a PySpark job to production?**
**Answer:**

*   Test on sample data in dev
    
*   Validate joins, counts, schema
    
*   Monitor memory on staging
    
*   Add alerting and audit logging
    
*   Review partitioning and file sizes
    

**Tip:** Run at real scale in QA once before prod push.

✅ **Module 5 – Cloud Integration + Airflow + CI/CD Readiness****Focus:** Cross-platform PySpark engineering — S3/Snowflake/Kafka integration, Airflow orchestration, Databricks Workflows, monitoring & alerting, production resilience, and CI/CD pipelines.
