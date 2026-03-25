# Mid-Level Data Analyst Engineer Assessment

**Duration:** 90 minutes | **Total Questions:** 10 | **Each question has one correct answer**

---

## Q1. You need to identify the second highest salary in each department from an `employees` table with columns `(emp_id, dept_id, salary)`. Which query correctly returns this?

**A)** `SELECT dept_id, MAX(salary) as second_highest FROM employees GROUP BY dept_id`  
**B)** `SELECT dept_id, salary FROM employees ORDER BY salary DESC LIMIT 1,1 PARTITION BY dept_id`  
**C)** `SELECT dept_id, salary FROM (SELECT dept_id, salary, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) as rnk FROM employees) WHERE rnk = 2`  
**D)** `SELECT dept_id, salary FROM employees WHERE salary < (SELECT MAX(salary) FROM employees) GROUP BY dept_id`  
**E)** `SELECT DISTINCT dept_id, LAG(salary, 1) OVER (PARTITION BY dept_id ORDER BY salary DESC) as second_highest FROM employees`

---

## Q2. An index exists on `customers(last_name, first_name, created_date)`. Which query cannot use this index for seeking (index seek)?

**A)** `WHERE last_name = 'Smith' AND first_name = 'John'`  
**B)** `WHERE last_name = 'Smith'`  
**C)** `WHERE first_name = 'John' AND created_date > '2024-01-01'`  
**D)** `WHERE last_name = 'Smith' AND created_date > '2024-01-01'`  
**E)** `WHERE last_name LIKE 'Sm%' AND first_name = 'John'`

---

## Q3. You need to merge two pandas DataFrames with different shapes. One has 1M rows with columns `(id, timestamp, value)` and another has 100K rows with columns `(id, category)`. The merge should be on `id`. Which approach handles this most memory-efficiently?

**A)** `pd.merge(df_large, df_small, on='id')` — pandas automatically uses the smaller DataFrame as the right side  
**B)** Convert both to SQL tables and perform the join there, then bring back to pandas  
**C)** Set `id` as index on both and use `join()` method  
**D)** Use `merge` with `sort=False` and explicitly specify `how` parameter based on business requirements  
**E)** A and D together are most efficient

---

## Q4. A DataFrame has missing values encoded as various forms: `'N/A'`, `'NULL'`, `''`, `None`, and `NaN`. You need to treat all these as missing before analysis. Which pandas operation correctly handles this?

**A)** `df.replace(['', 'N/A', 'NULL', None], np.nan)`  
**B)** `df.replace(['', 'N/A', 'NULL'], np.nan, regex=False)` then `df.fillna()`  
**C)** `df.replace(r'', np.nan, regex=True)` to match all empty variations  
**D)** `df.replace(['', 'N/A', 'NULL', None], np.nan, inplace=True)`  
**E)** None of these handle all cases correctly

---

## Q5. A product manager claims that changing the button color from blue to green will increase click-through rate by 5%. You design an A/B test with α=0.05 and 80% power. After running the test, p-value = 0.03. Which interpretation is correct?

**A)** The change increases CTR by 5% — the claim is proven  
**B)** There's only a 3% probability the result is due to chance  
**C)** We reject the null hypothesis; the difference is statistically significant at α=0.05, but we haven't proven the 5% magnitude claim  
**D)** The test needs more data before making any conclusion  
**E)** C is correct; the effect size estimation should accompany the hypothesis test

---

## Q6. You run 20 different A/B tests simultaneously (each at α=0.05) and find 3 statistically significant results. How do you interpret this?

**A)** All 3 results are real effects worth implementing  
**B)** We should apply multiple testing correction (e.g., Bonferroni) before making decisions  
**C)** Running fewer tests would have given cleaner results  
**D)** B and C are both valid considerations for future experiments  
**E)** All of the above are relevant

---

## Q7. You're designing an A/B test for a notification system where treatment users receive more frequent push notifications. What is the most critical risk to plan for?

**A)** Novelty effect: users may be initially more engaged just because something changed  
**B)** Novelty effect and habituation: engagement may drop after initial spike as users become annoyed  
**C)** The test will take too long to reach significance  
**D)** Mobile push notifications have low baseline engagement  
**E)** B and C are the main concerns

---

## Q8. You need to calculate the minimum sample size for an A/B test where baseline conversion is 5%, minimum detectable effect is 0.5%, with 80% power and 95% significance. Which formula/components are correct?

**A)** Depends on z-scores for power (0.84) and significance (1.96), plus the variance formula for proportions  
**B)** Sample size = (z_α + z_β)² × (p₁(1-p₁) + p₂(1-p₂)) / (p₁-p₂)²  
**C)** This requires a calculator or statistical software; manual calculation is error-prone  
**D)** All of the above are correct components for sample size calculation  
**E)** Power analysis software is preferred over formula for complex designs

---

## Q9. In Apache Airflow, a DAG has three tasks: T1 (extract), T2 (transform), T3 (load). T2 fails. According to default Airflow behavior, what happens to T3?

**A)** T3 will run anyway since it doesn't depend on T2's output  
**B)** T3 will not run because Airflow automatically stops downstream tasks on failure  
**C)** Airflow retries T2 before deciding whether to run T3  
**D)** Depends on trigger rules configured for T3  
**E)** B and D are correct

---

## Q10. A team is choosing between BigQuery slot reservations vs flat-rate pricing vs on-demand. They have consistent daily workloads with 70% utilization of reserved capacity. Which pricing model is most cost-effective?

**A)** BigQuery Editions (formerly flat-rate) for consistent, predictable workloads; better than on-demand at this utilization  
**B)** On-demand since 70% utilization suggests over-provisioning  
**C)** Slot reservations for the 70% baseline, with on-demand for bursts  
**D)** All can be analyzed via BigQuery pricing calculator; utilization isn't the only factor  
**E)** C is most cost-effective; reserve for baseline, burst on-demand

---
