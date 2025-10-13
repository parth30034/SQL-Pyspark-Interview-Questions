✅ TOPIC-WISE Q&A SET
--------------------

### ⚙️ Optimization + Performance

**1\. Your Spark job takes 40 minutes on 200 million records. What do you check first to optimize?****Answer:**

*   Skewed joins? (check distribution)
    
*   Partition count vs executors (too many/few?)
    
*   Are .cache() or .persist() overused?
    
*   Are wide transformations causing shuffles?
    
*   Avoid .collect() and .count() before filtering
    

**Tools:** Use Spark UI to inspect job stages and tasks.

**2\. What is the difference between repartition() and coalesce()? When to use each?****Answer:**

*   repartition(n): Increases or reshuffles partitions (full shuffle)
    
*   coalesce(n): Reduces partitions (avoids full shuffle)
    

**Use Case:**Use coalesce() before writing small output files.Use repartition() before wide joins or groupBy to balance load.

### 🧩 Partitioning Strategy

**3\. How do you decide partition column while writing a Delta table?****Answer:**Choose columns with:

*   High cardinality (but not too high)
    
*   Frequent filter access
    
*   Even distribution
    

**Example:**Good → event\_date, regionBad → user\_id (too many), status (too few)

**4\. What is partition pruning in Spark? How does it affect performance?****Answer:**Partition pruning means Spark reads only relevant partitions based on filter.

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.filter("event_date = '2024-12-01'")   `

**Effect:** Reduces data scanned → faster queries**Caution:** Avoid functions like DATE(event\_date) on partition column — it disables pruning.

### 🧠 UDFs & Native Functions

**5\. When should you avoid using UDFs in PySpark?****Answer:**Avoid UDFs when:

*   You can use native SQL functions (concat, when, regexp\_replace)
    
*   Performance is critical (UDFs break Catalyst optimizer)
    

**If Needed:** Use **Pandas UDFs** for vectorized performance.

**6\. How do you register a UDF and use it inside a SQL query in PySpark?****Answer:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   from pyspark.sql.functions import udf  from pyspark.sql.types import StringType  def mask_email(email):      return email.split("@")[0] + "@*****"  spark.udf.register("mask_email", mask_email, StringType())  spark.sql("SELECT mask_email(email) FROM users").show()   `

**Use Case:** Custom transformations not supported natively.

### 🪟 Window Functions

**7\. What is a window function in Spark and where is it useful?****Answer:**Window functions operate over a group (partition) of rows.

**Use Cases:**

*   Ranking (RANK, ROW\_NUMBER)
    
*   Rolling average
    
*   Deduplication
    

**Example:** Get latest transaction per user

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   from pyspark.sql.window import Window  from pyspark.sql.functions import row_number  windowSpec = Window.partitionBy("user_id").orderBy(col("txn_time").desc())  df.withColumn("rn", row_number().over(windowSpec)).filter("rn = 1")   `

**8\. What’s the difference between ROW\_NUMBER(), RANK(), and DENSE\_RANK()?****Answer:**

*   ROW\_NUMBER(): Always unique, sequential
    
*   RANK(): Skips rank if duplicate (1, 2, 2, 4...)
    
*   DENSE\_RANK(): Doesn’t skip (1, 2, 2, 3...)
    

**Use Case:**ROW\_NUMBER() → DeduplicationRANK() → Leaderboards

### 🧱 Delta + I/O Handling

**9\. How do you overwrite only one partition in a Delta table using PySpark?****Answer:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.write \    .format("delta") \    .mode("overwrite") \    .option("replaceWhere", "event_date = '2024-12-01'") \    .save("/mnt/delta/sales")   `

**Why Important:** Prevents full-table overwrite → saves time and cost.

**10\. What is the impact of writing small files in Delta Lake and how do you resolve it?****Answer:****Problem:** Too many small files → high metadata overhead → slow reads

**Fix:**

*   Use coalesce(n) before write
    
*   Run OPTIMIZE on the table periodically
    

**Bonus:** Monitor file size distribution in \_delta\_log.