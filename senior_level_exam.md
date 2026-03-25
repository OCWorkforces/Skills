# Senior Data Analyst Engineer Assessment

**Duration:** 120 minutes | **Total Questions:** 10 | **Each question has one correct answer**

---

## Q1. A fintech company processing 50M+ transactions daily is evaluating data infrastructure for fraud detection. They need sub-second latency for scoring and historical analysis for regulatory audits. Their team has strong SQL expertise but limited in stream processing. Which architecture would you recommend?

**A)** Traditional data warehouse with nightly batch processing for historical analysis and a separate Redis cache for real-time scoring  
**B)** Kappa architecture with a stream processing layer (Kafka + Flink) for real-time scoring and materialized views for historical queries  
**C)** Lambda architecture with separate batch and speed layers, using Apache Spark for batch and Kafka Streams for real-time  
**D)** Data lakehouse approach using Delta Lake with change data capture for near-real-time and Spark for batch historical analysis  
**E)** Pure streaming architecture with Kafka as the source of truth and Elasticsearch for both real-time and historical queries

---

## Q2. A retail chain with 500 stores is modernizing their data infrastructure after an acquisition doubled their footprint. They have legacy POS systems with different schemas, a 15-year-old Teradata warehouse, and limited cloud experience. The new CDO wants to "move fast and use AI." What is the most appropriate first step?

**A)** Lift-and-shift the Teradata warehouse to a cloud data warehouse and immediately start ML initiatives  
**B)** Implement a cloud-native data lake with automated schema detection to handle the diverse POS data quickly  
**C)** Conduct a data inventory assessment, prioritize integration requirements, and build a phased migration roadmap  
**D)** Deploy an iPaaS solution for real-time POS integration and a dashboard tool for immediate visibility  
**E)** Hire external consultants to build a greenfield modern data stack while training internal team

---

## Q3. A data analyst notices a query scanning 2TB of data when a filtered result should return only 50MB. The WHERE clause filters on a column with B-tree index. The EXPLAIN plan shows a full table scan despite the index. The most likely cause is:

**A)** The optimizer chose index scan due to outdated statistics  
**B)** The WHERE clause uses a function on the indexed column  
**C)** The table hasn't been analyzed recently and the optimizer misestimates cardinality  
**D)** The query joins are causing the optimizer to choose hash join over index lookup  
**E)** The index is on a low-cardinality column making index scan inefficient compared to table scan

---

## Q4. A query joining a 1 billion row transaction table to a 10 million row customer table performs poorly despite both tables being partitioned by date. The query filters on transaction date and joins on customer_id. EXPLAIN shows partition pruning happening but the join operation taking hours. What is the most likely bottleneck?

**A)** Cross-partition joins are causing shuffle operations that overwhelm network bandwidth  
**B)** The customer table partition pruning is not happening, causing full table scan  
**C)** The join is not partition-compatible, forcing redistribution of both large tables  
**D)** The query is running with insufficient memory allocation causing spill to disk  
**E)** The customer_id column lacks an index causing nested loop joins

---

## Q5. A company wants to measure the causal impact of a new recommendation algorithm on user engagement. They can't randomize users due to network effects—users who get the algorithm influence users who don't through social connections. Which experimental design is MOST appropriate?

**A)** Cluster randomization: randomize at the network/community level instead of individual user level  
**B)** Switchback experiment: alternate treatment and control on the same users over time  
**C)** Instrumental variables: find a variable that affects algorithm exposure but not engagement directly  
**D)** Difference-in-differences: compare engagement before/after in treatment vs. control regions  
**E)** Synthetic control: construct a counterfactual from similar users not exposed to the algorithm

---

## Q6. A data scientist builds a churn prediction model achieving 95% accuracy. When deployed, it performs worse than random at identifying churners. The most likely explanation is:

**A)** Training data was imbalanced (95% non-churners) and accuracy metric masked poor minority class performance  
**B)** The model overfitted to training data due to too many features  
**C)** Production data has concept drift where churn patterns have changed  
**D)** The training pipeline had data leakage that inflated training metrics  
**E)** The model's probability threshold was set incorrectly for the production population

---

## Q7. A data catalog implementation has good metadata coverage (descriptions, owners, schemas) but poor lineage coverage—only 30% of tables have lineage documented. Why does lineage matter MORE than static metadata for governance?

**A)** Lineage enables impact analysis: understanding downstream effects of changes  
**B)** Lineage allows tracing data quality issues to root causes  
**C)** Regulatory requirements (GDPR, CCPA) mandate lineage for audit trails  
**D)** Lineage enables column-level security by tracking sensitive data flow  
**E)** All of the above; lineage is fundamental to data trust and governance

---

## Q8. A multinational company subject to GDPR needs to delete a specific customer's data across a data lake containing 50PB of data in 10,000 tables. The legal team has verified the customer wants their data deleted. What is the MOST practical approach?

**A)** Drop and recreate all tables after identifying and deleting the customer's records  
**B)** Implement a technical deletion process that marks records as deleted and excludes them from queries  
**C)** Use the data lake's built-in GDPR deletion capabilities if available (e.g., Delta Lake time travel exclusion)  
**D)** Identify critical PII tables first, delete there, and implement tombstoning for derived tables  
**E)** C and D: use lakehouse deletion capabilities for governed tables and a prioritized approach for critical PII

---

## Q9. A senior data analyst is mentoring two junior analysts. One produces technically excellent work but struggles to explain findings to non-technical stakeholders. The other is excellent at communication but produces analysis with methodological flaws. How should the senior analyst approach this?

**A)** Focus first on the communication skills gap since business impact depends on communicating insights  
**B)** Focus first on methodological rigor since flawed analysis can lead to bad business decisions  
**C)** Address both in parallel with different interventions: pair the methodical analyst with storytelling exercises, pair the communicator with methodology training  
**D)** Escalate to management about the mismatched hires and need for role clarification  
**E)** Accept that analysts have different strengths and let them specialize accordingly

---

## Q10. A data engineering team is experiencing "pipeline debt"—dozens of scheduled jobs with implicit dependencies, undocumented assumptions, and no clear ownership. They want to modernize. What is the MOST important first step?

**A)** Implement a workflow orchestrator (Airflow, Prefect, Dagster) to make dependencies explicit  
**B)** Create a data catalog documenting all pipelines, their owners, and dependencies  
**C)** Conduct a pipeline inventory and assign clear ownership to each pipeline  
**D)** Implement data contracts between pipeline stages to formalize interfaces  
**E)** Build automated data quality tests for each pipeline to catch issues early

---
