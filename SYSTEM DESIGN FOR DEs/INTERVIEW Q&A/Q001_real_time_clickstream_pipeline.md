# Q001 — Real-Time Clickstream Processing Pipeline

### 📖 Context
An online retail company wants to analyze **user clickstream data** in near real-time to power dashboards for marketing and product analytics.  
The system should handle millions of events per hour and provide **aggregated metrics** (e.g., active users, top pages, conversion funnels).

---

### ❓ Question
Design a **real-time data pipeline** to process and analyze user clickstream data.  
Your solution should address:  
- Data ingestion (from web/app servers)  
- Real-time processing and aggregations  
- Storage layer for raw and processed data  
- Querying and visualization for dashboards  
- Scalability and fault-tolerance considerations  

---

### ✅ Expected Output (Architecture Components)
Your design should include components like:  
- **Producers** (web/app servers emitting events)  
- **Message Queue/Stream** (Kafka / Kinesis / PubSub)  
- **Stream Processing** (Spark Structured Streaming / Flink)  
- **Data Lake / Warehouse** (S3 + Delta Lake / BigQuery / Snowflake / Redshift)  
- **Serving Layer** (Presto/Trino / BI Tool / API)  

---

### 💡 Hints
- Consider **partitioning** strategy for scalability.  
- Think about **exactly-once processing** guarantees.  
- Use **Medallion Architecture (Bronze, Silver, Gold)** for incremental refinement.  

---

### 🗝️ Example Answer (High-Level)
- **Ingestion**: Use Kafka topics to collect clickstream events from web/app servers.  
- **Processing**: Spark Structured Streaming job consumes Kafka, performs sessionization, aggregates metrics (e.g., active users, top pages).  
- **Storage**: Raw data stored in **Bronze (S3/Delta Lake)**, cleaned data in **Silver**, aggregated data in **Gold**.  
- **Serving**: Expose Gold tables to BI dashboards via **Presto** or **Tableau/Looker**.  
- **Scalability**: Kafka partitions + autoscaling Spark clusters handle spikes.  
- **Fault tolerance**: Enable checkpointing and exactly-once semantics in Spark.  

---

### 🔥 Diagram (Optional)
```
[Web/App Servers] --> [Kafka] --> [Spark Structured Streaming] --> [Delta Lake (Bronze/Silver/Gold)] --> [Presto/BI Dashboard]
```

---

## 🌟 Tips for Contributors
- Keep your design answers **high-level** (don’t write too much code).  
- Use **architecture diagrams** in ASCII, Mermaid, or images if possible.  
- Focus on **trade-offs** (latency vs cost, batch vs streaming).  
