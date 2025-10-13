### ✅ TOPIC-WISE Q&A SET

#### 🧩 PySpark Basics + Execution Logic

**1\. What is lazy evaluation in PySpark? Why is it important?****Answer:**PySpark only builds the DAG (execution plan) until an **action** is called (e.g., .count(), .show()).

**Why important:**

*   Optimizes entire pipeline before running
    
*   Reduces unnecessary computations
    

**Real Use Case:** You can chain multiple filters and joins, and Spark optimizes them into a single job.

**2\. What is the difference between an action and a transformation? Give examples.****Answer:**

*   **Transformation:** Lazy, returns a new DataFrame (e.g., .filter(), .select())
    
*   **Action:** Triggers execution (e.g., .count(), .collect())
    

**Example:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df_filtered = df.filter("age > 30")  # transformation  df_filtered.show()                   # action   `

**3\. When should you use .collect() vs .show() in PySpark?****Answer:**

*   .show(): Displays limited rows (default 20) in console
    
*   .collect(): Brings all rows into driver — risky for big data
    

**Best Practice:** Avoid .collect() in production on large datasets.

#### 🔗 Joins & Real-World Examples

**4\. How does PySpark handle joins internally? What types do you commonly use?****Answer:**Joins in Spark are **shuffles** (unless broadcasted).

**Common types:** inner, left, right, outer, semi, anti**Real Example:**Use left join to get all customers even if they have no transactions.Avoid skew by broadcasting small tables.

**5\. You have skewed data in a join. How can you fix it in PySpark?****Answer:**

*   Use broadcast() for smaller table
    
*   Apply **salting** on join keys
    
*   Use **skew join hint:**
    

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df1.join(df2.hint("skew"), "id")   `

**Tip:** Always check distribution before joining large datasets.

#### 🧮 DataFrame Operations in ETL

**6\. How do you add a new column in PySpark based on existing ones?****Answer:**Use withColumn() and when(), otherwise() for conditions:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   from pyspark.sql.functions import when  df = df.withColumn("status", when(df.age > 30, "Senior").otherwise("Junior"))   `

**7\. What’s the difference between select() and selectExpr()?****Answer:**

*   select(): Uses column objects
    
*   selectExpr(): Accepts SQL expressions as strings
    

**Use Case:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.selectExpr("name", "salary * 1.2 as updated_salary")   `

**8\. What is cache() vs persist() in PySpark? When to use which?****Answer:**

*   cache(): Stores in memory (default level)
    
*   persist(): Can store to memory + disk or other storage levels
    

**Use Case:**Use cache() when you reuse the DataFrame multiple times and it fits in memory.

#### 🍰 Reading & Writing Data

**9\. How do you write a DataFrame to a Delta table in append mode?****Answer:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df.write.format("delta").mode("append").save("/mnt/delta/sales")   `

**Pro Tip:** Use partitioning to optimize performance.

**10\. How do you read a CSV file with header and infer schema in PySpark?****Answer:**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   df = spark.read.option("header", "true") \                 .option("inferSchema", "true") \                 .csv("path/to/file.csv")   `

**Best Practice:** For production, **define schema explicitly** using StructType for better stability.