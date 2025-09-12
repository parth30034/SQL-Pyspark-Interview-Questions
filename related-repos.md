# 📚 Related Repositories & Resources

A curated list of repos and resources that align with building a **SQL + PySpark scenario-based question bank**.

---

## 1. [mohankrishna02 / interview-scenerios-spark-sql](https://github.com/mohankrishna02/interview-scenerios-spark-sql)
- **What it’s good for**: Real-world style scenario prompts and interview-style question formats.  
- **What’s missing**: Some questions lack runnable notebooks or full sample datasets — good candidate to augment.  
- **Recommendation**: Fork as your **`scenario/`** folder; add `CREATE TABLE` + sample `INSERT` scripts per question.  
- **Tags**: `scenarios`, `sql`, `pyspark`, `interview`  
- **Difficulty**: `Intermediate → Advanced`

---

## 2. [kailasneupane / LeetCode-SQL-PySpark-Solutions](https://github.com/kailasneupane/LeetCode-SQL-PySpark-Solutions)
- **What it’s good for**: Runnable notebooks showing SQL → PySpark translations; strong example of keeping solutions runnable.  
- **What’s missing**: Primarily LeetCode problems (not long multi-step scenarios).  
- **Recommendation**: Borrow the **notebook layout** and runnable examples; use same pattern for scenario solutions.  
- **Tags**: `sql`, `pyspark`, `leetcode`, `notebooks`  
- **Difficulty**: `Basic → Intermediate`

---

## 3. [areibman / pyspark_exercises](https://github.com/areibman/pyspark_exercises)
- **What it’s good for**: Clear, small exercises covering PySpark API fundamentals (great for *basic* level).  
- **What’s missing**: Fewer long end-to-end scenarios.  
- **Recommendation**: Use to fill **basic → intermediate** levels and to teach primitives before scenario problems.  
- **Tags**: `pyspark`, `dataframe`, `basics`, `api`  
- **Difficulty**: `Basic`

---

## 4. [Devinterview-io / apache-spark-interview-questions](https://github.com/Devinterview-io/apache-spark-interview-questions)
- **What it’s good for**: Curated Spark interview Q&A (conceptual coverage).  
- **What’s missing**: Limited runnable code.  
- **Recommendation**: Include short **concept-check cards** in each scenario (e.g., *why broadcasting here?*) using this repo as source.  
- **Tags**: `concepts`, `interview`, `spark`, `q&a`  
- **Difficulty**: `Basic → Advanced (conceptual)`

---

## 5. [gabridego / spark-exercises](https://github.com/gabridego/spark-exercises)
- **What it’s good for**: Simulates real data-engineering scenarios for BI & pipelines.  
- **What’s missing**: May need extra datasets or multiple-solution variants.  
- **Recommendation**: Adapt their **scenario templates** into your `scenario/advanced/` folder.  
- **Tags**: `scenarios`, `etl`, `bi`, `pipelines`  
- **Difficulty**: `Intermediate → Advanced`

---

## 6. [VirtusLab / pyspark-workshop](https://github.com/VirtusLab/pyspark-workshop)
- **What it’s good for**: Workshop-style notebooks and exercises; interactive learning format.  
- **Recommendation**: Use their **workshop-notebook style** for instructor-led sessions or group practice runs.  
- **Tags**: `workshop`, `pyspark`, `training`, `notebooks`  
- **Difficulty**: `Basic → Intermediate`

---

## 7. [dhruv-agg / pyspark_practice](https://github.com/dhruv-agg/pyspark_practice)
- **What it’s good for**: Solved PySpark data engineering exercises (time-series, cleaning, transforms).  
- **Recommendation**: Copy **test cases & sample datasets** into `datasets/` and convert them into `CREATE TABLE` scripts.  
- **Tags**: `practice`, `pyspark`, `data-cleaning`, `transformations`  
- **Difficulty**: `Intermediate`

---

## 8. [UrbanInstitute / pyspark-tutorials](https://github.com/UrbanInstitute/pyspark-tutorials)
- **What it’s good for**: Well-documented tutorials with real datasets and reproducible examples.  
- **Recommendation**: Use their **documentation style** for writing problem statements + context.  
- **Tags**: `tutorials`, `pyspark`, `real-data`, `documentation`  
- **Difficulty**: `Basic → Intermediate`

---

## 9. [LeetCode-SQL / leetcode-sql](https://github.com/topics/leetcode-sql)
- **What it’s good for**: Large collection of SQL problems you can adapt to PySpark.  
- **Recommendation**: Pick challenging SQL problems and expand into **multi-step scenario prompts** (ingest → transform → aggregate → serve).  
- **Tags**: `sql`, `leetcode`, `problem-bank`, `practice`  
- **Difficulty**: `Basic → Advanced`

---

## 10. Additional Reading / Curated Lists
- **Sources**: Medium articles, interview prep blogs, curated Spark Q&A guides.  
- **Recommendation**: Copy the **common pitfalls**, **expected optimizations**, and **evaluation rubric** into each scenario for better guidance.  
- **Tags**: `reading`, `interview`, `optimizations`, `best-practices`  
- **Difficulty**: `All levels`
