# Mid-Level Data Analyst Engineer Assessment (100 Questions)

---

## ADVANCED SQL (20 Questions)

---

### Q1. A Data Analyst needs to calculate the running total of sales for each product category, ordered by date, and reset the running total to zero whenever the product category changes. Which SQL technique is most appropriate?

**A)** Using a self-join with aggregate functions
**B)** Using a window function with `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` with `PARTITION BY category`
**C)** Using multiple correlated subqueries grouped by date
**D)** Using a recursive CTE with category boundaries
**E)** Using `GROUP BY ROLLUP(category, date)`

**Answer:** B — Using a window function with `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` with `PARTITION BY category`

**Explanation:** Window functions with `PARTITION BY` allow calculating running totals within each partition (category) while resetting at partition boundaries. The `ROWS BETWEEN` clause defines the window frame correctly for cumulative sums.

**Topic:** Advanced SQL

---

### Q2. Your analytics team reports that a query joining `orders` (10M rows) to `customers` (500K rows) takes 45 seconds. The execution plan shows a Hash Match join. Which optimization approach would likely yield the most improvement?

**A)** Adding a `WHERE` clause to filter both tables before joining
**B)** Replacing the Hash Match with a Merge Join by creating indexes on join keys
**C)** Using `SELECT DISTINCT` on the customers table to eliminate duplicate joins
**D)** Converting the query to use a subquery instead of a join
**E)** Increasing the server's RAM allocation

**Answer:** B — Replacing the Hash Match with a Merge Join by creating indexes on join keys

**Explanation:** Creating indexes on the join keys allows the optimizer to choose a Merge Join, which is typically faster than Hash Match for large datasets when the data is already sorted. Hash Match is efficient when tables are large but unsorted.

**Topic:** Advanced SQL

---

### Q3. You need to identify the **second highest salary** in each department from an `employees` table with columns `(emp_id, dept_id, salary)`. Which query correctly returns this?

**A)** `SELECT dept_id, MAX(salary) as second_highest FROM employees GROUP BY dept_id`
**B)** `SELECT dept_id, salary FROM employees ORDER BY salary DESC LIMIT 1,1 PARTITION BY dept_id`
**C)** `SELECT dept_id, salary FROM (SELECT dept_id, salary, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) as rnk FROM employees) WHERE rnk = 2`
**D)** `SELECT dept_id, salary FROM employees WHERE salary < (SELECT MAX(salary) FROM employees) GROUP BY dept_id`
**E)** `SELECT DISTINCT dept_id, LAG(salary, 1) OVER (PARTITION BY dept_id ORDER BY salary DESC) as second_highest FROM employees`

**Answer:** C — `SELECT dept_id, salary FROM (SELECT dept_id, salary, DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) as rnk FROM employees) WHERE rnk = 2`

**Explanation:** Using `DENSE_RANK()` as a window function with `PARTITION BY dept_id` correctly identifies the 2nd highest salary within each department. `DENSE_RANK()` avoids gaps in ranking when there are ties. Note: `RANK()` is NOT interchangeable here — with ties, RANK() jumps to 3 while DENSE_RANK() correctly returns 2.

**Topic:** Advanced SQL

---

### Q4. In an e-commerce database, you need to compare each customer's **current month spending** to their **same month from the previous year** (SPLY - Same Period Last Year). Which approach efficiently returns both values in a single scan of the data?

**A)** Self-join the orders table twice with date range conditions
**B)** Use `LAG(spending, 12) OVER (PARTITION BY customer_id ORDER BY month)` assuming monthly granularity
**C)** Use a CTE to calculate current year and previous year aggregates separately, then join
**D)** Use `FIRST_VALUE` and `LAST_VALUE` window functions
**E)** Use a recursive CTE to traverse the date hierarchy

**Answer:** B — Use `LAG(spending, 12) OVER (PARTITION BY customer_id ORDER BY month)` assuming monthly granularity

**Explanation:** `LAG()` window function can look back a specific number of periods (12 months in this case) within a partitioned dataset, allowing efficient same-period-last-year comparison without joins or multiple scans.

**Topic:** Advanced SQL

---

### Q5. A slowly changing dimension table uses **Type 2 SCD** to track historical employee department assignments. Which query pattern correctly returns only the **current active department** for each employee?

**A)** `WHERE end_date IS NULL`
**B)** `WHERE is_current = 'Y'`
**C)** Using `ROW_NUMBER() OVER (PARTITION BY employee_id ORDER BY start_date DESC) = 1`
**D)** Both A and B are correct depending on schema design
**E)** `WHERE DATEDIFF(day, start_date, end_date) = (SELECT MAX(DATEDIFF(day, start_date, end_date)) FROM employees)`

**Answer:** D — Both A and B are correct depending on schema design

**Explanation:** Both approaches are valid depending on how the SCD Type 2 is implemented. Some schemas use `end_date IS NULL` to indicate current records, while others use a boolean flag like `is_current`. Both correctly identify the current active record.

**Topic:** Advanced SQL

---

### Q6. Your query uses a correlated subquery that executes for each row of the outer query, causing poor performance. Which transformation is most likely to improve performance?

**A)** Replace with a `CROSS APPLY` or `OUTER APPLY`
**B)** Add more indexes on all columns
**C)** Convert the subquery to a `SELECT DISTINCT`
**D)** Remove all `WHERE` conditions from the subquery
**E)** Wrap the subquery in a CTE without modification

**Answer:** A — Replace with a `CROSS APPLY` or `OUTER APPLY`

**Explanation:** `CROSS APPLY` (or `OUTER APPLY` in SQL Server) can allow the optimizer to unnest correlated subqueries and use more efficient execution plans, often converting them to joins that execute in a single pass. Note: `CROSS APPLY`/`OUTER APPLY` is T-SQL specific. The ANSI SQL equivalent is `LATERAL JOIN` (used in PostgreSQL).

**Topic:** Advanced SQL

---

### Q7. You need to calculate the **percentage contribution** of each product to its category's total revenue, along with a running percentage within each category ordered by revenue. Which SQL achieves both in a single query?

**A)** `SELECT product, category, revenue, revenue/SUM(revenue) OVER () as pct_total, SUM(revenue) OVER (ORDER BY revenue) as running_total FROM sales`
**B)** `SELECT product, category, revenue, 100.0*revenue/SUM(revenue) OVER (PARTITION BY category) as pct_of_category, 100.0*SUM(revenue) OVER (PARTITION BY category ORDER BY revenue ROWS UNBOUNDED PRECEDING)/SUM(revenue) OVER (PARTITION BY category) as running_pct FROM sales`
**C)** `SELECT product, category, revenue, revenue/(SELECT SUM(revenue) FROM sales WHERE category = s.category) as pct_of_category FROM sales s`
**D)** Using a CTE for category totals joined to the main query
**E)** Both B and D achieve the same result efficiently

**Answer:** A — `SELECT product, category, revenue, revenue/SUM(revenue) OVER () as pct_total, SUM(revenue) OVER (ORDER BY revenue) as running_total FROM sales`

**Explanation:** Both approaches correctly calculate percentage of category and running percentage within category. Option B does it in a single query using window functions, while Option D uses a CTE approach which the optimizer can often materialize and reuse.

**Topic:** Advanced SQL

---

### Q8. An index exists on `customers(last_name, first_name, created_date)`. Which query **cannot** use this index for seeking (index seek)?

**A)** `WHERE last_name = 'Smith' AND first_name = 'John'`
**B)** `WHERE last_name = 'Smith'`
**C)** `WHERE first_name = 'John' AND created_date > '2024-01-01'`
**D)** `WHERE last_name = 'Smith' AND created_date > '2024-01-01'`
**E)** `WHERE last_name LIKE 'Sm%' AND first_name = 'John'`

**Answer:** C — `WHERE first_name = 'John' AND created_date > '2024-01-01'`

**Explanation:** A composite index follows the leftmost prefix rule. Since `first_name` is the second column without `last_name` in the condition, the index cannot be used for seeks on `first_name` alone. The index can be used for `last_name` alone or `last_name` + `first_name`, or `last_name` + `created_date`.

**Topic:** Advanced SQL

---

### Q9. You need to find **gaps and islands** in a sequence of production batch IDs that should be sequential but occasionally have missing numbers. Which SQL technique is most appropriate?

**A)** Using `ROW_NUMBER()` to compare actual vs expected sequence
**B)** Using a self-join to identify adjacent pairs
**C)** Using a recursive CTE with `GENERATE_SERIES` or similar
**D)** Using `LAG` to calculate differences between consecutive IDs and `GROUP BY` those differences to identify islands
**E)** All of the above approaches can work depending on the database system

**Answer:** A — Using `ROW_NUMBER()` to compare actual vs expected sequence

**Explanation:** Gap-island problems are classic SQL challenges with multiple valid solutions. The approach depends on the specific database system (PostgreSQL vs SQL Server vs Oracle) and data characteristics. Row number comparison, LAG-based grouping, and recursive CTEs are all valid strategies.

**Topic:** Advanced SQL

---

### Q10. In a data warehouse fact table with 500 million rows, a business user reports that their query filtering on `transaction_date` is slow despite having an index. The table uses **partitioning by month**. What's likely the issue?

**A)** The index on `transaction_date` is not being used
**B)** The query is hitting too many partitions (querying across many months)
**C)** The fact table needs to be normalized
**D)** The index needs to be a covering index
**E)** Both B and D are likely contributing factors

**Answer:** A — The index on `transaction_date` is not being used

**Explanation:** When querying across many partitions, partition pruning may not be efficient. Additionally, if the index is not a covering index (includes all columns needed by the query), the optimizer may choose a full scan instead. Adding included columns to make it a covering index can dramatically improve performance.

**Topic:** Advanced SQL

---

### Q11. You need to calculate **median order value** for each sales region. Which SQL construct correctly handles this for an even number of values (returns average of two middle values)?

**A)** `SELECT region, AVG(amount) FROM orders GROUP BY region`
**B)** Using percentile functions like `PERCENTILE_CONT(0.5)` with `GROUP BY region`
**C)** Using `ROW_NUMBER()` to find middle rows and averaging them
**D)** Using `MEDIAN()` function if available in your database
**E)** B, C, or D depending on database system and data characteristics

**Answer:** A — `SELECT region, AVG(amount) FROM orders GROUP BY region`

**Explanation:** Different databases handle median differently. `PERCENTILE_CONT(0.5)` in SQL Server/PostgreSQL calculates a continuous median (interpolating). Some databases have a native `MEDIAN()` function. For even counts, manual calculation with `ROW_NUMBER()` averages the two middle values. The "best" answer depends on the specific environment.

**Topic:** Advanced SQL

---

### Q12. A query joins 5 large tables and takes 3 minutes. The execution plan shows a **Table Spool** operator appearing multiple times. What does this typically indicate?

**A)** The query is using parallelism efficiently
**B)** The optimizer is materializing intermediate result sets, often due to complex join ordering or repeated access to the same data
**C)** The tables need to be denormalized
**D)** There is a missing index causing scans
**E)** The query is being executed multiple times

**Answer:** B — The optimizer is materializing intermediate result sets, often due to complex join ordering or repeated access to the same data

**Explanation:** Table Spools materialize intermediate results, often appearing when the optimizer determines that re-scanning a base table would be more expensive than storing a temporary copy. This can indicate suboptimal join ordering or queries that reference the same table multiple times in different ways.

**Topic:** Advanced SQL

---

### Q13. Which statement correctly describes the difference between **RANK()**, **DENSE_RANK()**, and **ROW_NUMBER()** window functions?

**A)** All three return the same values when there are no ties
**B)** `RANK()` leaves gaps, `DENSE_RANK()` has no gaps, `ROW_NUMBER()` is unique per row
**C)** `RANK()` and `DENSE_RANK()` can return duplicate values, `ROW_NUMBER()` cannot
**D)** Both A and C are correct
**E)** RANK() can return duplicate values but never leaves gaps

**Answer:** D — Both A and C are correct

**Explanation:** Option E is incorrect because RANK() DOES leave gaps (1, 1, 3). Option D is correct: A is true (no ties = same values), and C is true (RANK and DENSE_RANK can return duplicates when there are ties, while ROW_NUMBER cannot). B correctly describes the gap behavior. The key distinction is that RANK() and DENSE_RANK() handle ties differently, but both can assign the same rank to different rows.

**Topic:** Advanced SQL

---

### Q14. You're writing a recursive CTE to generate an **organizational hierarchy** from an employees table with `emp_id` and `manager_id`. The base case should start with:

**A)** All employees who have no manager (manager_id IS NULL)
**B)** All employees with `emp_level = 1`
**C)** The entire employees table
**D)** All employees ordered by hire_date
**E)** A single dummy row to start recursion

**Answer:** A — All employees who have no manager (manager_id IS NULL)

**Explanation:** In an organizational hierarchy, the recursion starts from the root nodes—employees who have no manager (typically CEO or top-level executives). The recursive part then joins to find each employee's direct reports until no more are found.

**Topic:** Advanced SQL

---

### Q15. A **materialized view** is created to pre-aggregate daily sales data. The business now wants **real-time or near-real-time** sales dashboards. What is the most appropriate next step?

**A)** Refresh the materialized view more frequently
**B)** Use dynamic partitioning to split the view into smaller pieces
**C)** Migrate to a live query approach with proper indexing, or use an incremental materialized view refresh strategy
**D)** Add more columns to the materialized view
**E)** Convert to a regular view

**Answer:** C — Migrate to a live query approach with proper indexing, or use an incremental materialized view refresh strategy

**Explanation:** More frequent refreshes of materialized views can increase maintenance overhead and still not provide true real-time data. For near-real-time dashboards, either use live queries with appropriate indexes or implement incremental/fast refresh strategies that update only changed partitions.

**Topic:** Advanced SQL

---

### Q16. You need to calculate **quarter-over-quarter growth rate** for revenue, handling cases where prior quarter revenue was zero or negative. Which SQL expression correctly handles division by zero?

**A)** `CASE WHEN prev_qtr_rev = 0 THEN NULL ELSE (curr_qtr_rev - prev_qtr_rev) / prev_qtr_rev * 100 END`
**B)** `NULLIF(prev_qtr_rev, 0)` to wrap denominator, then use `IFNULL` for display
**C)** `DIV0NULL(curr_qtr_rev - prev_qtr_rev, prev_qtr_rev) * 100` (Snowflake syntax)
**D)** All of the above approaches prevent division by zero
**E)** Only A and B are database-agnostic

**Answer:** A — `CASE WHEN prev_qtr_rev = 0 THEN NULL ELSE (curr_qtr_rev - prev_qtr_rev) / prev_qtr_rev * 100 END`

**Explanation:** `NULLIF` is ANSI SQL and works in most databases. `CASE` statements are universally supported. `DIV0NULL` is Snowflake-specific. All three prevent division by zero, but A and B are more portable across different database systems. For negative denominators, `NULLIF` also returns NULL, preventing the negative division. If the business wants to distinguish negative growth from missing data, wrap with additional CASE logic.

**Topic:** Advanced SQL

---

### Q17. The execution plan for your query shows a **Sort operator** with high estimated row size. Which technique might eliminate this sort?

**A)** Ensure data is already sorted by creating an index on the ORDER BY columns
**B)** Use `SELECT DISTINCT` to eliminate duplicates
**C)** Add a `WHERE` clause to reduce rows
**D)** Replace window functions with correlated subqueries
**E)** Both A and C are correct

**Answer:** A — Ensure data is already sorted by creating an index on the ORDER BY columns

**Explanation:** If the query requires sorted output (for `ORDER BY`, `SORT`, or window functions with `ORDER BY`), creating an index on those columns allows the optimizer to read data in sorted order, eliminating the expensive Sort operator.

**Topic:** Advanced SQL

---

### Q18. You need to implement **idempotent** upsert logic (INSERT or UPDATE if exists) for a dimension table. Which approach ensures multiple executions produce the same result?

**A)** `MERGE INTO target USING source ON target.key = source.key WHEN MATCHED THEN UPDATE WHEN NOT MATCHED THEN INSERT`
**B)** `INSERT ON CONFLICT DO UPDATE` (PostgreSQL)
**C)** Use a transaction with separate DELETE and INSERT statements
**D)** Both A and B are idempotent if the operation completes successfully
**E)** Only B is idempotent; A requires additional logic

**Answer:** D — Both A and B are idempotent if the operation completes successfully

**Explanation:** Both `MERGE` and `ON CONFLICT DO UPDATE` are idempotent operations. If the operation runs multiple times with the same source data, the final state of the target table will be identical after each complete execution. The key is ensuring the entire operation completes atomically.

**Topic:** Advanced SQL

---

### Q19. A **denormalized fact table** has duplicate transaction IDs due to a join with a bridge table for many-to-many customer relationships. Which technique helps count **unique transactions** accurately?

**A)** `COUNT(DISTINCT transaction_id)`
**B)** `COUNT(*) / COUNT(DISTINCT bridge_key)` as an approximation
**C)** Pre-aggregate transactions before joining to the bridge table
**D)** Use a subquery with `ROW_NUMBER()` to deduplicate before aggregation
**E)** C or D depending on whether exact counts or performance is prioritized

**Answer:** A — `COUNT(DISTINCT transaction_id)`

**Explanation:** If exact unique counts are required and performance allows, deduplication via subquery or CTE before joining is correct. If the bridge table represents a true many-to-many relationship and approximation is acceptable, pre-aggregation before the join is more performant.

**Topic:** Advanced SQL

---

### Q20. Your analytics query needs to calculate **moving average of the last 7 days** for each store, excluding weekends. The data only contains transactions for business days. Which window frame specification is correct?

**A)** `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW`
**B)** `ROWS BETWEEN 7 PRECEDING AND CURRENT ROW`
**C)** `RANGE BETWEEN 6 PRECEDING AND CURRENT ROW`
**D)** `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
**E)** The answer depends on whether you want a simple row count or calendar-based window

**Answer:** A — `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW`

**Explanation:** If the business requirement is "last 7 calendar days," and data only contains business days, you cannot use simple row offsets. You need either calendar-based logic (comparing dates) or a row-based approach if the business only cares about the last 7 transaction days. The specification must match the business definition.

**Topic:** Advanced SQL

---

## ADVANCED PYTHON FOR DATA ANALYSIS (15 Questions)

---

### Q21. You need to merge two pandas DataFrames with different shapes. One has 1M rows with columns `(id, timestamp, value)` and another has 100K rows with columns `(id, category)`. The merge should be on `id`. Which approach handles this most memory-efficiently?

**A)** `pd.merge(df_large, df_small, on='id')` — pandas automatically uses the smaller DataFrame as the right side
**B)** Convert both to SQL tables and perform the join there, then bring back to pandas
**C)** Set `id` as index on both and use `join()` method
**D)** Use `merge` with `sort=False` and explicitly specify `how` parameter based on business requirements
**E)** A and D together are most efficient

**Answer:** D — Use `merge` with `sort=False` and explicitly specify `how` parameter based on business requirements

**Explanation:** Pandas' `merge` automatically broadcasts the smaller DataFrame. Explicitly specifying `how` (left/right/inner/outer) avoids creating unnecessary rows. Using `sort=False` avoids expensive sorting if not needed. For very large merges, database or Spark-based approaches may be necessary.

**Topic:** Advanced Python for Data Analysis

---

### Q22. A DataFrame has a column `event_json` containing JSON strings like `'{"action": "click", "properties": {"x": 100, "y": 200}}'`. You need to extract `action` and `properties.x` into separate columns. Which approach is most efficient?

**A)** Using `apply` with `json.loads` and dictionary access
**B)** Using `pd.json_normalize` after converting strings to dictionaries
**C)** Using vectorized string operations with `str.extract` for simple cases
**D)** Using `pandas.io.json.json_normalize` with appropriate record_path
**E)** The best approach depends on whether the JSON structure is consistent

**Answer:** A — Using `apply` with `json.loads` and dictionary access

**Explanation:** For consistent nested JSON, `pd.json_normalize()` is fastest. For inconsistent or deeply nested JSON, `apply` with `json.loads` provides flexibility. If only extracting top-level keys, string operations may suffice. The optimal approach depends on data consistency and depth of nesting.

**Topic:** Advanced Python for Data Analysis

---

### Q23. You need to process 10GB of CSV files in chunks to calculate monthly aggregates while minimizing memory usage. Which pattern correctly implements this?

**A)** Process entire file, write to database, repeat for next file
**B)** Use `pd.read_csv(chunksize=100000)` and accumulate results in a dictionary of DataFrames
**C)** Use `pd.read_csv(chunksize=100000)`, calculate aggregates per chunk, store in a results list, then combine at the end
**D)** Convert CSV to Parquet first, then process
**E)** Use multiprocessing to parallelize CSV reading

**Answer:** C — Use `pd.read_csv(chunksize=100000)`, calculate aggregates per chunk, store in a results list, then combine at the end

**Explanation:** Chunked processing with per-chunk aggregation minimizes memory usage by never holding the full dataset. Accumulating partial aggregates (counts, sums) and combining them at the end is the standard pattern for memory-efficient large file processing.

**Topic:** Advanced Python for Data Analysis

---

### Q24. You need to schedule a Python data pipeline to run daily at 2 AM, with automatic retry on failure and alerting on repeated failures. Which approach is most appropriate for a mid-size analytics team?

**A)** Cron job with shell script wrapper
**B)** Python script with try/except and logging, triggered by systemd timer
**C)** Apache Airflow DAG with retry logic and Slack/email alerts
**D)** GitHub Actions scheduled workflow
**E)** AWS Lambda with CloudWatch scheduled event

**Answer:** C — Apache Airflow DAG with retry logic and Slack/email alerts

**Explanation:** For a team with multiple pipelines, dependencies, and need for retry/alerts, Airflow provides orchestration, scheduling, retry logic, alerting, and UI for monitoring—all difficult to implement reliably with cron/shell scripts. Airflow is the industry standard for data pipeline orchestration.

**Topic:** Advanced Python for Data Analysis

---

### Q25. A DataFrame has 50 columns and 1M rows. You need to apply a complex transformation that takes 10 seconds per row. Using `df.apply(row_transform, axis=1)` will take weeks. Which parallelization approach is most appropriate?

**A)** Use `joblib.Parallel` with `delayed` to parallelize across rows
**B)** Use `pandas.DataFrame.apply` with `parallelized=True`
**C)** Use `modin` or `dask` DataFrames for automatic parallelization
**D)** Vectorize the operation if possible, or use `swifter` to auto-vectorize
**E)** C or D depending on whether the transformation can be vectorized

**Answer:** A — Use `joblib.Parallel` with `delayed` to parallelize across rows

**Explanation:** If the transformation can be vectorized, `swifter` or manual vectorization is fastest. If not vectorizable and the operation is CPU-bound, `modin`/`dask` or `joblib` can parallelize. The choice depends on whether vectorization is feasible and the size of computation.

**Topic:** Advanced Python for Data Analysis

---

### Q26. You need to consume a REST API that returns paginated JSON data with 10,000 pages. The API has rate limiting (100 requests/minute). Which approach ensures complete data retrieval efficiently?

**A)** Sequential requests with `time.sleep(0.6)` between calls
**B)** Use `asyncio` with `aiohttp` for concurrent requests within rate limits
**C)** Use `concurrent.futures.ThreadPoolExecutor` with semaphore to control request rate
**D)** B or C with exponential backoff for 429 (rate limit) responses
**E)** All of the above can work depending on infrastructure constraints

**Answer:** A — Sequential requests with `time.sleep(0.6)` between calls

**Explanation:** All approaches can work. Sequential with sleep is simplest but slowest. Async/threaded approaches with semaphore control concurrency. Exponential backoff is essential for handling unexpected rate limiting. The choice depends on infrastructure and how the API responds to concurrent requests.

**Topic:** Advanced Python for Data Analysis

---

### Q27. A DataFrame has missing values encoded as various forms: `'N/A'`, `'NULL'`, `''`, `None`, and `NaN`. You need to treat all these as missing before analysis. Which pandas operation correctly handles this?

**A)** `df.replace(['', 'N/A', 'NULL', None], np.nan)`
**B)** `df.replace(['', 'N/A', 'NULL'], np.nan, regex=False)` then `df.fillna()`
**C)** `df.replace(r'', np.nan, regex=True)` to match all empty variations
**D)** `df.replace(['', 'N/A', 'NULL', None], np.nan, inplace=True)`
**E)** None of these handle all cases correctly

**Answer:** A — `df.replace(['', 'N/A', 'NULL', None], np.nan)`

**Explanation:** `df.replace()` with a list of values to replace handles all these cases. Note that `None` must be in the list separately since it's not a string. Using `inplace=True` is deprecated in newer pandas versions; the assignment pattern is preferred.

**Topic:** Advanced Python for Data Analysis

---

### Q28. You need to export a processed DataFrame to a data warehouse. The DataFrame has 5M rows and 50 columns. Performance is critical. Which approach is fastest?

**A)** `df.to_csv()` and bulk load to warehouse
**B)** `df.to_parquet()` then use warehouse's COPY command
**C)** `df.to_sql()` with `method='multi'` for batch inserts
**D)** Convert to numpy arrays and use warehouse's direct API for bulk insert
**E)** B is typically fastest for cloud data warehouses

**Answer:** A — `df.to_csv()` and bulk load to warehouse

**Explanation:** For cloud data warehouses (Snowflake, BigQuery, Redshift), writing to Parquet and using the warehouse's native bulk load utility (COPY, LOAD) is typically fastest because it bypasses row-by-row INSERT and uses columnar storage optimization.

**Topic:** Advanced Python for Data Analysis

---

### Q29. You have a function that processes individual customer records and may fail on malformed data. You need to process millions of records while continuing on errors. Which error handling pattern is most appropriate?

**A)** Wrap entire DataFrame processing in try/except
**B)** Use `df.apply()` with `errors='coerce'` and handle NaN results
**C)** Use a logging-based approach with per-record try/except, collecting errors and valid results separately
**D)** Use `swifter` library for automatic error handling
**E)** Use `pandarallel` for automatic error propagation

**Answer:** C — Use a logging-based approach with per-record try/except, collecting errors and valid results separately

**Explanation:** For large-scale processing where individual record failures shouldn't stop the entire job, per-record try/except with error collection (dead letter queue pattern) is most robust. This allows processing to continue and provides a log of failures for later investigation/retry.

**Topic:** Advanced Python for Data Analysis

---

### Q30. You need to read a deeply nested XML file (50GB) into pandas for analysis. Which approach is most memory-efficient?

**A)** `pd.read_xml()` with `iterparse` for streaming
**B)** Use `xml.etree.ElementTree` with custom generator yielding records
**C)** Use `lxml` with iterparse and yield dictionaries
**D)** Convert XML to JSON first, then read with `pd.json_normalize`
**E)** B or C using a streaming/generator approach

**Answer:** A — `pd.read_xml()` with `iterparse` for streaming

**Explanation:** For large XML files, using `iterparse` from `xml.etree` or `lxml` as a generator yields records one at a time, building DataFrames in batches. This avoids loading the entire file into memory. Pure `pd.read_xml()` loads the entire structure.

**Topic:** Advanced Python for Data Analysis

---

### Q31. A DataFrame has a datetime column `event_time` stored as UTC. You need to convert to three timezones (US/Eastern, US/Pacific, Europe/London) for different stakeholders. Which approach is most efficient?

**A)** Use `df['event_time'].dt.tz_convert()` three times creating three new columns
**B)** Use `df['event_time'].dt.tz_localize('UTC').dt.tz_convert()` for each timezone
**C)** Convert once to a timezone-aware dtype, then create derived columns using `dt.tz_convert()`
**D)** Use vectorized operations with `pytz` or `zoneinfo`
**E)** C and D are equally efficient

**Answer:** C — Convert once to a timezone-aware dtype, then create derived columns using `dt.tz_convert()`

**Explanation:** Once the datetime is timezone-aware, deriving additional timezone columns is efficient because it doesn't require re-parsing. Using the `dt` accessor for each conversion is the standard pandas approach. `zoneinfo` (Python 3.9+) is preferred over `pytz` for newer code.

**Topic:** Advanced Python for Data Analysis

---

### Q32. You need to perform **memory optimization** on a DataFrame with 100M rows and 30 columns. Which technique provides the most significant memory reduction?

**A)** Convert string columns to `category` dtype using `df[col].astype('category')`
**B)** Downcast numeric columns using `pd.to_numeric(df[col], downcast='integer/float')`
**C)** Select only needed columns before processing
**D)** Use `df.select_dtypes` to identify and optimize column types
**E)** All of the above combined provide optimal memory reduction

**Answer:** A — Convert string columns to `category` dtype using `df[col].astype('category')`

**Explanation:** Memory optimization is cumulative. Converting strings to categories can reduce 90%+ memory for low-cardinality columns. Downcasting numerics saves 50-75%. Selecting only needed columns avoids loading unused data. Combined approaches provide the greatest reduction.

**Topic:** Advanced Python for Data Analysis

---

### Q33. You need to calculate year-over-year growth for 10,000 products across 5 years of daily data. The naive approach creates a large cross-join. Which pandas pattern is most efficient?

**A)** Use `merge_asof` with directional `allow_exact_matches=False`
**B)** Set index to `(product_id, date)` and use `groupby`, then `shift`
**C)** Use `pivot_table` then calculate YoY on the pivoted data
**D)** Use SQL window functions via `pandasql`
**E)** B or C depending on whether you need daily granularity in output

**Answer:** A — Use `merge_asof` with directional `allow_exact_matches=False`

**Explanation:** For time-series comparisons, `groupby().shift()` with the appropriate period (365 days) is efficient. `pivot_table` can work but may create very wide tables for 10,000 products. SQL `merge_asof` is excellent when data is already sorted and you need nearest-date matching.

**Topic:** Advanced Python for Data Analysis

---

### Q34. You need to validate data quality across 50 CSV files with different schemas. Which approach creates the most maintainable validation framework?

**A)** Use `assert` statements after each file load
**B)** Use a JSON schema definition per file type and `jsonschema` library for validation
**C)** Use `great_expectations` with expectation suites
**D)** Write a validation function with per-column rules and apply across files
**E)** C or D for a production data pipeline

**Answer:** B — Use a JSON schema definition per file type and `jsonschema` library for validation

**Explanation:** For production pipelines, `great_expectations` provides data documentation, profiling, and validation with a nice UI. For simpler needs, a custom validation function with declarative rules is lightweight and maintainable. Raw `assert` statements don't scale well.

**Topic:** Advanced Python for Data Analysis

---

### Q35. You need to read a Parquet file that was written with `compression='snappy'` by Spark. Your pandas version is 2.0+. Which statement is correct?

**A)** pandas automatically reads Snappy-compressed Parquet files without additional dependencies
**B)** You need `pyarrow` or `fastparquet` installed to read Snappy Parquet
**C)** pandas cannot read Spark-written Parquet files
**D)** You need to decompress the file before reading
**E)** A and B are both true depending on the engine parameter

**Answer:** B — You need `pyarrow` or `fastparquet` installed to read Snappy Parquet

**Explanation:** pandas uses `pyarrow` (default in 2.0+) or `fastparquet` engines to read Parquet files. Both support Snappy compression. If neither is installed, pandas falls back to `pyarrow` which needs to be installed separately.

**Topic:** Advanced Python for Data Analysis

---

## STATISTICS & HYPOTHESIS TESTING (15 Questions)

---

### Q36. A product manager claims that changing the button color from blue to green will increase click-through rate by 5%. You design an A/B test with α=0.05 and 80% power. After running the test, p-value = 0.03. Which interpretation is correct?

**A)** The change increases CTR by 5% — the claim is proven
**B)** There's only a 3% probability the result is due to chance
**C)** We reject the null hypothesis; the difference is statistically significant at α=0.05, but we haven't proven the 5% magnitude claim
**D)** The test needs more data before making any conclusion
**E)** C is correct; the effect size estimation should accompany the hypothesis test

**Answer:** B — There's only a 3% probability the result is due to chance

**Explanation:** A significant p-value (0.03 < 0.05) means we reject the null hypothesis of no difference. However, it doesn't prove the 5% magnitude claim—that's a point estimate that should be reported with a confidence interval. Effect size and practical significance are separate from statistical significance.

**Topic:** Statistics & Hypothesis Testing

---

### Q37. You run a **two-sample t-test** and get a p-value of 0.06. The business stakeholders ask if this means the new feature is "definitely not working." How do you respond?

**A)** No, 0.06 is close to 0.05, and with more data or a one-tailed test, it would be significant
**B)** At α=0.05, we fail to reject the null, but there's a 6% probability the result is due to chance
**C)** The result is "not statistically significant," but we should examine the confidence interval for practical significance
**D)** All of the above are partially correct; the nuanced answer depends on context
**E)** Both B and C are correct interpretations

**Answer:** C — The result is "not statistically significant," but we should examine the confidence interval for practical significance

**Explanation:** Option B describes the incorrect interpretation (p-value isn't the probability of chance). The correct interpretation is that at α=0.05 we fail to reject the null, but examining the confidence interval reveals the range of plausible effect sizes, including both practical significance and the possibility of no effect.

**Topic:** Statistics & Hypothesis Testing

---

### Q38. You're analyzing the relationship between **customer tenure** (months) and **lifetime value** (dollars). The scatter plot shows a curved pattern. A linear regression gives R²=0.45. Which approach is most appropriate?

**A)** Apply **log transformation** to the target variable and re-fit
**B)** Use **polynomial regression** to capture the curve
**C)** Use **Spearman correlation** instead of Pearson to assess relationship strength
**D)** Both A and B are valid approaches; validation on holdout data determines the best model
**E)** All of the above should be tried and compared via cross-validation

**Answer:** B — Use **polynomial regression** to capture the curve

**Explanation:** For non-linear relationships, log transformation (if LTV is right-skewed) and polynomial regression are both valid. Spearman correlation assesses monotonic relationships and should be used when linearity assumptions are violated. Cross-validation on holdout data determines which model generalizes best.

**Topic:** Statistics & Hypothesis Testing

---

### Q39. You run 20 different A/B tests simultaneously (each at α=0.05) and find 3 statistically significant results. How do you interpret this?

**A)** All 3 results are real effects worth implementing
**B)** We should apply **multiple testing correction** (e.g., Bonferroni) before making decisions
**C)** Running fewer tests would have given cleaner results
**D)** B and C are both valid considerations for future experiments
**E)** All of the above are relevant

**Answer:** B — We should apply **multiple testing correction** (e.g., Bonferroni) before making decisions

**Explanation:** With 20 tests at α=0.05, we'd expect ~1 false positive by chance alone (20 × 0.05 = 1). Multiple testing correction (Bonferroni, Benjamini-Hochberg) adjusts the threshold. Planning for fewer, higher-quality tests is better practice than running many simultaneous tests.

**Topic:** Statistics & Hypothesis Testing

---

### Q40. A **95% confidence interval** for average order value is [$47, $53]. Which statement is most accurate?

**A)** There's a 95% probability the true mean is between $47 and $53
**B)** If we repeated the experiment many times, 95% of confidence intervals would contain the true mean
**C)** 95% of individual order values fall within this range
**D)** Both A and B are equivalent interpretations
**E)** Both B and C are correct

**Answer:** B — If we repeated the experiment many times, 95% of confidence intervals would contain the true mean

**Explanation:** The frequentist interpretation of a confidence interval is that if we repeated sampling many times, 95% of the constructed CIs would contain the true parameter. Option A is a Bayesian interpretation (which requires a prior). Option C describes a prediction interval or percentile range, not a CI for the mean.

**Topic:** Statistics & Hypothesis Testing

---

### Q41. You're comparing conversion rates across 4 website variants (A, B, C, D) using **ANOVA**. The F-test is significant (p < 0.05). What's the appropriate next step?

**A)** Conclude that all variants are different from each other
**B)** Use **post-hoc pairwise comparisons** (e.g., Tukey HSD) to identify which specific pairs differ
**C)** Run t-tests on all pairs without correction since ANOVA was significant
**D)** Increase sample size and rerun ANOVA
**E)** Both B and C are valid depending on the penalty for false positives

**Answer:** B — Use **post-hoc pairwise comparisons** (e.g., Tukey HSD) to identify which specific pairs differ

**Explanation:** A significant ANOVA indicates at least one group mean differs, but doesn't tell us which. Post-hoc tests like Tukey HSD correct for multiple comparisons and identify specific pairs. Running uncorrected t-tests increases Type I error rate.

**Topic:** Statistics & Hypothesis Testing

---

### Q42. A **chi-square test** for independence between customer segment and product preference returns p-value = 0.001. What does this tell you?

**A)** Customer segment **causes** product preference
**B)** There's a statistically significant association between segment and preference, but causation cannot be inferred
**C)** The effect size is large
**D)** Both B and C are supported by the chi-square test
**E)** We need to run a regression to understand the relationship

**Answer:** B — There's a statistically significant association between segment and preference, but causation cannot be inferred

**Explanation:** Chi-square tests detect association, not causation. A significant result means the variables are not independent. Effect size (Cramer's V) should be reported separately. Causation requires experimental design (randomization) or careful causal inference methods.

**Topic:** Statistics & Hypothesis Testing

---

### Q43. You fit a **logistic regression** model to predict customer churn. The model coefficients are all positive. A stakeholder asks why some coefficients are negative in another model. What explains the difference?

**A)** The models used different target definitions
**B)** The models had different feature sets or the features were scaled differently
**C)** Multicollinearity can flip coefficient signs
**D)** All of the above are possible explanations
**E)** Negative coefficients always indicate data quality issues

**Answer:** D — All of the above are possible explanations

**Explanation:** Coefficient signs depend on target definition (higher=good vs higher=bad), feature scaling, feature selection, multicollinearity, and the specific optimization outcome. Models should be compared on predictive performance, not coefficient sign alone.

**Topic:** Statistics & Hypothesis Testing

---

### Q44. A/B test result: Control conversion = 2.1%, Treatment conversion = 2.4%. The business wants to launch the treatment if lift > 10%. Is this significant and meaningful?

**A)** (2.4-2.1)/2.1 = 14.3% lift > 10%, launch it
**B)** Need p-value and confidence interval to determine statistical significance before business decision
**C)** The 14.3% lift is the observed effect; need to check if it's practically significant given business constraints
**D)** B and C are both necessary for a complete decision
**E)** Need Bayesian posterior probability to make decision

**Answer:** D — B and C are both necessary for a complete decision

**Explanation:** The observed 14.3% lift is a point estimate. The confidence interval might include values below 10% (if high variance). Statistical significance (p-value) and practical significance (does 10%+ lift justify the change?) are separate considerations. Both inform the business decision.

**Topic:** Statistics & Hypothesis Testing

---

### Q45. You need to calculate the **minimum sample size** for an A/B test where baseline conversion is 5%, minimum detectable effect is 0.5%, with 80% power and 95% significance. Which formula/components are correct?

**A)** Depends on z-scores for power (0.84) and significance (1.96), plus the variance formula for proportions
**B)** Sample size = (z_α + z_β)² × (p₁(1-p₁) + p₂(1-p₂)) / (p₁-p₂)²
**C)** This requires a calculator or statistical software; manual calculation is error-prone
**D)** All of the above are correct components for sample size calculation
**E)** Power analysis software is preferred over formula for complex designs

**Answer:** D — All of the above are correct components for sample size calculation

**Explanation:** Sample size formulas for proportions use z-scores for the chosen alpha and power. The pooled variance formula assumes equal sample sizes. For complex designs (sequential testing, multiple comparisons), simulation-based power calculators are more accurate than basic formulas.

**Topic:** Statistics & Hypothesis Testing

---

### Q46. A regression model predicting sales has **VIF (Variance Inflation Factor)** values of 8.2, 9.1, 1.2, and 1.1 for four predictors. What does this indicate?

**A)** Multicollinearity is present among the first two variables
**B)** Variables 3 and 4 are independent of other predictors
**C)** The model needs variable selection or regularization
**D)** All of the above are supported by the VIF values
**E)** All of the above and VIF > 5 indicates problematic multicollinearity

**Answer:** C — The model needs variable selection or regularization

**Explanation:** VIF > 5 (some use > 10) indicates multicollinearity that may bias coefficient estimates. Variables 1 and 2 have high VIFs suggesting redundancy. Variables 3 and 4 have low VIFs. Remedies include removing redundant variables, combining them, or using regularization (Ridge, Lasso).

**Topic:** Statistics & Hypothesis Testing

---

### Q47. You're comparing two regression models: Model A (R²=0.72, 5 predictors) vs Model B (R²=0.74, 12 predictors). Which model is "better"?

**A)** Model B because higher R²
**B)** Model A because it has fewer predictors (parsimony)
**C)** Need to compare using adjusted R² or cross-validated performance
**D)** Need to examine residuals for model adequacy
**E)** C and D are necessary for model comparison

**Answer:** C — Need to compare using adjusted R² or cross-validated performance

**Explanation:** Adjusted R² penalizes added predictors and may favor Model A. Cross-validation assesses out-of-sample predictive performance. Residual analysis checks model assumptions. R² alone doesn't determine which model generalizes better.

**Topic:** Statistics & Hypothesis Testing

---

### Q48. A data scientist claims their **Bayesian model** shows "95% probability the new feature increases conversions." A frequentist responds this is invalid. Who's correct?

**A)** The frequentist is correct; Bayesian and frequentist interpretations differ
**B)** The Bayesian can report posterior probability given prior and likelihood
**C)** The statement is valid if interpreted as "given the data and prior, probability is 95%"
**D)** Both A and C are correct interpretations
**E)** The frequentist is always more rigorous

**Answer:** D — Both A and C are correct interpretations

**Explanation:** The Bayesian posterior probability is valid under Bayesian interpretation (probability is degree of belief). The frequentist critique is that probability describes long-run frequency, not a single event's probability. Both frameworks can be valid; the interpretation must match the framework used.

**Topic:** Statistics & Hypothesis Testing

---

### Q49. You run a **paired t-test** comparing delivery times before and after a process change for the same 500 customers. The p-value is 0.04. Which assumption is critical for valid inference?

**A)** The difference scores are normally distributed (or n>30 by CLT)
**B)** The samples are independent
**C)** Homogeneity of variance between groups
**D)** Both A and B are necessary for paired t-test validity
**E)** Both A and C are necessary for paired t-test validity

**Answer:** A — The difference scores are normally distributed (or n>30 by CLT)

**Explanation:** The paired t-test's validity depends on the differences being normally distributed (or n large enough for CLT to apply). Paired means samples are matched, not independent—making independence assumption different from two-sample tests. Homogeneity of variance is a two-sample concern, not paired.

**Topic:** Statistics & Hypothesis Testing

---

### Q50. A marketing campaign targets users based on a **propensity score** model. After targeting, the observed conversion rate for the targeted group is higher. Why might this **not** prove the model works?

**A)** Selection bias: targeted users were already more likely to convert
**B)** The model was validated on the same data used for evaluation
**C)** Confounding variables weren't controlled
**D)** All of the above are threats to causal inference
**E)** A/B testing on the model output is needed to validate causal effect

**Answer:** C — Confounding variables weren't controlled

**Explanation:** Observational post-hoc comparison doesn't establish causation. Users self-selected into the targeted group based on characteristics that also predict conversion. True validation requires randomized A/B testing where the model's propensity score determines treatment assignment.

**Topic:** Statistics & Hypothesis Testing

---

## DATA MODELING & SCHEMA DESIGN (10 Questions)

---

### Q51. You're designing a data model for a retail analytics system. The business needs to analyze **sales by product, store, date, and customer** with fast aggregations. Which schema design is most appropriate?

**A)** Fully normalized 3NF design for data integrity
**B)** **Star schema** with a central fact table and dimension tables for product, store, date, customer
**C)** Snowflake schema with normalized dimension tables
**D)** A single wide table with all attributes
**E)** B for query performance; C for storage efficiency

**Answer:** B — **Star schema** with a central fact table and dimension tables for product, store, date, customer

**Explanation:** Star schema is optimal for analytical queries because it minimizes joins (fact table directly connects to dimension tables), enables fast aggregations, and is intuitive for business users. Snowflake normalizes dimensions, saving storage but adding joins.

**Topic:** Data Modeling & Schema Design

---

### Q52. In a **star schema** for sales analytics, the product dimension has 50,000 products but only 20 active categories. The category name is stored directly in the product dimension table. What design issue does this represent?

**A)** This is correct star schema design
**B)** **Denormalization** of the category attribute, which is acceptable and even recommended in star schemas
**C)** This violates normalization rules and should be fixed
**D)** Category should be a separate dimension table (snowflake)
**E)** This will cause storage issues

**Answer:** B — **Denormalization** of the category attribute, which is acceptable and even recommended in star schemas

**Explanation:** Denormalizing category into the product dimension is standard star schema practice. It reduces joins for common queries (sales by category), adds minimal storage cost, and doesn't cause update anomalies in dimensions which change slowly. This is appropriate analytical design.

**Topic:** Data Modeling & Schema Design

---

### Q53. A customer dimension table needs to track **changes in customer address** over time. The business needs to see historical orders with the address that was valid at order time. Which SCD (Slowly Changing Dimension) approach is appropriate?

**A)** SCD Type 1: overwrite the old address
**B)** SCD Type 2: add new row with new address and effective dates
**C)** SCD Type 3: add columns for previous address
**D)** Use a separate address history table with customer FK
**E)** B or D depending on query complexity and historical analysis needs

**Answer:** C — SCD Type 3: add columns for previous address

**Explanation:** Type 2 (new rows) is the classic approach for full historical tracking. A separate address history table (Type 4) also works and may be cleaner for complex address changes. Type 1 loses history. Type 3 only tracks one previous value. The choice depends on query patterns and how many historical changes matter.

**Topic:** Data Modeling & Schema Design

---

### Q54. A **fact table** has columns: `(order_key, customer_key, product_key, store_key, date_key, quantity, unit_price, discount)`. Which measures are **additive** vs **non-additive**?

**A)** quantity is additive; unit_price is non-additive (aggregates via AVG); discount is additive
**B)** quantity and discount are additive; unit_price is non-additive
**C)** quantity and unit_price are additive; discount is semi-additive
**D)** All are additive since they're numeric
**E)** Additivity depends on the grain of the fact table

**Answer:** A — quantity is additive; unit_price is non-additive (aggregates via AVG); discount is additive

**Explanation:** quantity and discount sum across dimensions (additive). unit_price is a unit-level attribute that's non-additive—summing prices across orders is meaningless. Note: this assumes discount is a dollar amount per line item. Percentage discounts would be non-additive. Pre-aggregated revenue (quantity × unit_price) would be additive.

**Topic:** Data Modeling & Schema Design

---

### Q55. You need to track **inventory levels at the end of each month** for each store-product combination. What's the most appropriate fact table type?

**A)** Transaction fact table (one row per inventory change event)
**B)** **Periodic snapshot fact table** (one row per month per store-product)
**C)** Accumulating snapshot fact table
**D)** Factless fact table
**E)** B or D depending on whether we need to track individual inventory transactions

**Answer:** B — **Periodic snapshot fact table** (one row per month per store-product)

**Explanation:** Periodic snapshots capture the state of measures at regular intervals. For monthly inventory levels, a periodic snapshot is appropriate. A transaction fact table would track every inventory movement (receiving, selling, returns). Accumulating snapshots track processes with defined start/end (like order fulfillment).

**Topic:** Data Modeling & Schema Design

---

### Q56. A **slowly changing dimension** uses Type 2 SCD. The business reports that query performance degrades as the dimension grows to millions of rows. Which optimization is most appropriate?

**A)** Partition the dimension table by active/inactive status
**B)** Create a **current view** that filters to active rows only for current-value queries
**C)** Implement a Type 1 override for rarely-changing attributes and Type 2 for frequently-changing ones
**D)** All of the above are valid optimization strategies
**E)** B and C are correct; Type 2 should only be used when historical changes are queried

**Answer:** C — Implement a Type 1 override for rarely-changing attributes and Type 2 for frequently-changing ones

**Explanation:** Partitioning the whole table may not help if queries still need historical data. A current-view/currect-flags approach speeds current-value queries. Hybrid Type 1/2 reduces row growth for attributes that don't need history tracking. The optimal strategy depends on query patterns (how often historical vs current data is accessed).

**Topic:** Data Modeling & Schema Design

---

### Q57. You need to design a **factless fact table** for tracking which marketing campaigns each customer was **exposed to** (impression data). Which design captures this?

**A)** `(campaign_key, customer_key, date_key, impression_flag)`
**B)** `(campaign_key, customer_key, date_key, time_of_impression)`
**C)** `(campaign_key, customer_key, date_key)` with no measures
**D)** Store impressions in a separate table from conversions
**E)** A or B depending on whether time-of-day matters for analysis

**Answer:** C — `(campaign_key, customer_key, date_key)` with no measures

**Explanation:** A factless fact table has no measures but captures relationships (campaign exposure). Whether you need time_of_impression depends on analysis requirements. If only daily exposure counts matter, (campaign_key, customer_key, date_key) suffices. If timing relative to conversion matters, include time or use a more granular design.

**Topic:** Data Modeling & Schema Design

---

### Q58. In a **snowflake schema**, the product dimension has: `product → subcategory → category → department`. The business wants to aggregate sales by department. Which is true?

**A)** Snowflake enables faster department-level aggregation than star
**B)** Star schema enables faster department-level aggregation than snowflake
**C)** Both perform equally after query optimization
**D)** The difference is negligible for modern databases
**E)** Depends on whether indexes exist on join keys

**Answer:** B — Star schema enables faster department-level aggregation than snowflake

**Explanation:** Star schema has fewer joins for cross-dimension aggregation. Snowflake normalizes dimensions into hierarchies, requiring additional joins for each level of the hierarchy (product → subcategory → category → department = 4 joins vs 1 join in star). The star's denormalized dimensions include department directly.

**Topic:** Data Modeling & Schema Design

---

### Q59. A **surrogate key** in a dimension table is preferred over a natural key because:

**A)** Surrogate keys are shorter and improve join performance
**B)** Surrogate keys are stable and don't change when source data changes
**C)** Natural keys from operational systems may have gaps or reuse patterns
**D)** All of the above are advantages of surrogate keys
**E)** Surrogate keys are always required by data warehouse standards

**Answer:** D — All of the above are advantages of surrogate keys

**Explanation:** Surrogate keys (typically auto-increment integers) provide stable, unique identifiers independent of source system changes. They enable handling duplicate/missing natural keys, improve join performance (smaller keys), and decouple the warehouse from operational system quirks. They're best practice but not always strictly required.

**Topic:** Data Modeling & Schema Design

---

### Q60. The business needs to analyze **orders that had all items shipped together** (same shipment_id) vs orders with partial shipments. The current schema has separate order and shipment tables. What change supports this analysis?

**A)** Add shipment_id to the order fact table or create a **bridge table** for order-to-shipment many-to-many relationship
**B)** Denormalize shipment data into the order table
**C)** Create a view joining orders and shipments
**D)** Add a flag to indicate if shipment was complete
**E)** A or D depending on whether individual shipment_ids are needed in analysis

**Answer:** C — Create a view joining orders and shipments

**Explanation:** If analysis only needs to know "complete vs partial shipments," a flag suffices. If analysis needs to examine specific shipment_ids (consolidation patterns, carrier performance), including shipment_key in the fact or using a bridge table is necessary. The design depends on analytical granularity needed.

**Topic:** Data Modeling & Schema Design

---

## A/B TESTING & EXPERIMENTATION (12 Questions)

---

### Q61. You're designing an A/B test for a **notification system** where treatment users receive more frequent push notifications. What is the most critical risk to plan for?

**A)** Novelty effect: users may be initially more engaged just because something changed
**B)** **Novelty effect and habituation**: engagement may drop after initial spike as users become annoyed
**C)** The test will take too long to reach significance
**D)** Mobile push notifications have low baseline engagement
**E)** B and C are the main concerns

**Answer:** B — **Novelty effect and habituation**: engagement may drop after initial spike as users become annoyed

**Explanation:** In notification experiments, users may initially engage more due to novelty, then engagement drops or they disable notifications entirely (habituation). This can mask the true long-term effect. Best practice: run experiments long enough to observe habituation, or use sequential testing to detect changing effects over time.

**Topic:** A/B Testing & Experimentation

---

### Q62. Your A/B test shows a **statistically significant negative impact** on revenue per user (-3%, p=0.02) but positive impact on engagement metrics (pageviews +8%, p=0.01). How do you interpret this?

**A)** The experiment should be stopped immediately
**B)** There's a trade-off between engagement and revenue; stakeholders need this information to make business decisions
**C)** Engagement increase will eventually lead to revenue increase
**D)** The result is contradictory and indicates a data quality issue
**E)** The negative revenue impact is more important since revenue is the primary metric

**Answer:** B — There's a trade-off between engagement and revenue; stakeholders need this information to make business decisions

**Explanation:** Statistical significance doesn't determine business priorities. The team needs to understand the trade-off: higher engagement with lower revenue could indicate users are browsing but not converting (maybe too many free trials?). This finding requires investigation and stakeholder discussion, not automatic stopping or launching.

**Topic:** A/B Testing & Experimentation

---

### Q63. You need to run an experiment on a website where user experience changes significantly after the first interaction (click). **Sequential testing** is being considered. What does sequential testing address?

**A)** It allows analyzing data as it comes in rather than waiting for fixed sample size
**B)** It maintains the desired false positive rate while allowing early stopping
**C)** It requires special adjustments (like alpha spending functions) to avoid inflated Type I error
**D)** All of the above are characteristics of sequential testing
**E)** It is more efficient than fixed-horizon testing

**Answer:** D — All of the above are characteristics of sequential testing

**Explanation:** Sequential testing (or adaptive testing) allows continuous monitoring and early stopping while controlling the overall Type I error rate using alpha-spending functions (like O'Brien-Fleming or Pocock boundaries). It can be more efficient if effects materialize early but can also increase complexity in analysis.

**Topic:** A/B Testing & Experimentation

---

### Q64. You want to test **10 different email subject lines** simultaneously. Which experimental design approach is most statistically appropriate?

**A)** Run 10 separate A/B tests with shared control group
**B)** Use **multivariate testing (MVT)** or **multi-armed bandit** approach
**C)** Use one-way ANOVA to compare all 10 variants
**D)** A/B test the top 2 from a small pilot, then scale
**E)** B, C, and D are all reasonable approaches with different trade-offs

**Answer:** C — Use one-way ANOVA to compare all 10 variants

**Explanation:** Separate tests with shared control (10 tests vs same control) requires multiple testing correction. MVT tests all combinations simultaneously but needs more traffic. Multi-armed bandits dynamically allocate traffic to better-performing arms. Pilot + follow-up is more practical for small traffic. Trade-offs depend on traffic volume and business context.

**Topic:** A/B Testing & Experimentation

---

### Q65. You launch a feature and see a +5% conversion lift in week 1, but by week 4 the lift is only +1%. What phenomenon might explain this?

**A)** **Novelty effect** wearing off as users become familiar with the feature
**B)** Seasonality differences between weeks
**C)** The treatment group has degraded experience over time
**D)** Both A and B are plausible explanations
**E)** All of A, B, and C should be investigated

**Answer:** C — The treatment group has degraded experience over time

**Explanation:** Declining treatment effects can indicate novelty effect (A), temporal confounders like seasonality or day-of-week effects (B), or even treatment group users adapting behavior (C). Investigation should examine temporal patterns, control group stability, and consider running experiments for full business cycles when possible.

**Topic:** A/B Testing & Experimentation

---

### Q66. A/B test power calculation indicates you need 10,000 users per variant. You have only 5,000 daily active users available. What's the best course of action?

**A)** Run the test for 2 days to reach 10,000 users per variant
**B)** Accept lower power (≈50%) or larger detectable effect size
**C)** Use a one-tailed test to effectively halve required sample
**D)** Increase the treatment effect size you're looking for
**E)** B or D are the practical options given traffic constraints

**Answer:** C — Use a one-tailed test to effectively halve required sample

**Explanation:** Lower power means higher chance of missing a real effect (Type II error). The detectable effect size increases when power decreases. One-tailed tests reduce required sample slightly but don't halve it. Running longer doesn't help if daily users are fixed—you'd need to run multiple days to get enough users. The real choice is business: accept larger minimum detectable effect or extend duration.

**Topic:** A/B Testing & Experimentation

---

### Q67. You run an A/B test on **pricing tiers**: Control = $9.99/month, Treatment = $12.99/month. The test shows p=0.08 for conversion rate. A stakeholder suggests continuing the test indefinitely until significant. What's wrong with this approach?

**A)** **Peeking problem**: monitoring until significance inflates Type I error rate
**B)** The test needs at least 30 days for monthly subscription patterns
**C)** $12.99 is too large a price jump for valid comparison
**D)** A and B are both valid concerns
**E)** All of the above are valid concerns

**Answer:** D — A and B are both valid concerns

**Explanation:** The peeking problem (continuously monitoring and stopping when p < 0.05) inflates false positive rates because the effective alpha increases with each look. Running for a full business cycle (30 days for subscriptions) addresses day-of-week and monthly patterns. The price jump magnitude affects whether the test is realistic but doesn't invalidate the statistical approach.

**Topic:** A/B Testing & Experimentation

---

### Q68. An experiment shows **significant interaction effect** between user age group and treatment (p=0.01). Young users respond positively, older users respond negatively. How do you proceed?

**A)** Report the interaction and recommend different treatments for different segments
**B)** Ignore the interaction since overall effect is the primary concern
**C)** Run separate experiments for each age segment to confirm
**D)** Both A and C are appropriate next steps
**E)** Use Bayesian approach to estimate segment-specific effects with uncertainty

**Answer:** D — Both A and C are appropriate next steps

**Explanation:** A significant interaction means treatment effect varies by segment. The honest approach is to report this finding and recommend segmented rollout or further validation with segment-specific experiments. Bayesian estimation provides uncertainty around segment effects, which is more informative than binary significance.

**Topic:** A/B Testing & Experimentation

---

### Q69. You want to measure the impact of an **offline change** (store layout redesign) on online behavior. Users cannot be randomly assigned to store layouts. Which approach provides the best causal estimate?

**A)** Randomized controlled experiment is impossible; use **observational study** with propensity score matching
**B)** Use difference-in-differences if you have comparable control stores
**C)** Use regression discontinuity if there's a clear cutoff for store eligibility
**D)** All of the above are valid quasi-experimental approaches
**E)** Causal inference is impossible without randomization

**Answer:** D — All of the above are valid quasi-experimental approaches

**Explanation:** When randomization isn't possible, quasi-experimental methods provide the best causal estimates. Propensity score matching mimics randomization. Difference-in-differences uses pre/post comparisons with control group. Regression discontinuity uses eligibility cutoffs. All are valid when designed carefully. Causal inference is harder but not impossible without randomization.

**Topic:** A/B Testing & Experimentation

---

### Q70. Your A/B test shows **p=0.04** for a 2% conversion lift. A Bayesian analyst provides 89% posterior probability that the treatment is better. How do you reconcile these?

**A)** They measure different things: frequentist p-value vs Bayesian posterior probability
**B)** The 89% is more informative for decision-making
**C)** They can both be correct since they're from different statistical frameworks
**D)** All of the above are accurate statements
**E)** The frequentist approach is more conservative

**Answer:** D — All of the above are accurate statements

**Explanation:** The frequentist p=0.04 means IF there's no true effect, there's a 4% chance of observing this data. The Bayesian 89% posterior means given the data and prior, there's 89% probability treatment is better. They answer different questions and can both be reported. Bayesian provides more direct business decision support.

**Topic:** A/B Testing & Experimentation

---

### Q71. An A/B test result shows treatment is better with p=0.03. The team launches the feature. 3 months later, the business metric returns to baseline. What went wrong?

**A)** Long-term effect differs from short-term measurement
**B)** Users may have adapted their behavior
**C)** The experiment only measured short-term metrics
**D)** All of the above are possible explanations
**E)** A/B tests cannot predict long-term effects

**Answer:** D — All of the above are possible explanations

**Explanation:** A/B tests measure average treatment effect during the experiment period. Long-term effects can differ due to user adaptation, market changes, competitive responses, or novelty effects wearing off. This is a fundamental limitation of experimentation. Consider longer experiment durations or longitudinal tracking post-launch.

**Topic:** A/B Testing & Experimentation

---

### Q72. You need to test a **feature that requires users to opt-in** (they choose to enable it). Simple random assignment won't work because non-opters contaminate the treatment group. Which design addresses this?

**A)** **Intent-to-treat (ITT)** analysis: analyze by original assignment regardless of actual use
**B)** **Treatment-on-treated (TOT)** analysis: only analyze users who actually used the feature
**C)** Use **encouragement design** with instrumental variables
**D)** All of the above provide different perspectives on treatment effect
**E)** A randomized encouragement design where users are encouraged but not forced to use the feature

**Answer:** C — Use **encouragement design** with instrumental variables

**Explanation:** In opt-in scenarios, pure randomization fails because treatment group includes non-compliers. ITT analyzes by assignment (preserves randomization but dilutes effect). TOT analyzes by actual use (bias from self-selection). Encouragement design (randomly encourage users to try) with ITT or IV analysis provides unbiased estimates of both intent-to-treat and causal effect of actual use.

**Topic:** A/B Testing & Experimentation

---

## ETL/ELT & DATA PIPELINES (10 Questions)

---

### Q73. In **Apache Airflow**, a DAG has three tasks: T1 (extract), T2 (transform), T3 (load). T2 fails. According to default Airflow behavior, what happens to T3?

**A)** T3 will run anyway since it doesn't depend on T2's output
**B)** T3 will **not run** because Airflow automatically stops downstream tasks on failure
**C)** Airflow retries T2 before deciding whether to run T3
**D)** Depends on trigger rules configured for T3
**E)** B and D are correct

**Answer:** C — Airflow retries T2 before deciding whether to run T3

**Explanation:** By default, Airflow's scheduler won't schedule T3 because T2 is upstream and failed. T3 will not run because Airflow automatically skips downstream tasks on upstream failure (default trigger rule is `all_success`). However, `trigger_rule` can be configured (e.g., `all_done`, `none_failed`, `always`) to change this behavior. Understanding trigger rules is critical for pipeline reliability.

**Topic:** ETL/ELT & Data Pipelines

---

### Q74. You're designing a data pipeline that ingests customer data from an external API that returns **changed records only** (CDC - Change Data Capture). How do you ensure **idempotency**?

**A)** Use the source system's `updated_at` timestamp to upsert records
**B)** Use a unique constraint on natural key and `ON CONFLICT DO UPDATE`
**C)** Store the last successful sync timestamp and query only for records updated since
**D)** All of the above are components of an idempotent CDC pipeline
**E)** CDC inherently provides idempotency if designed correctly

**Answer:** D — All of the above are components of an idempotent CDC pipeline

**Explanation:** True idempotency requires: (1) deterministic record identification using natural keys or timestamps, (2) upsert logic that produces the same final state regardless of how many times the operation runs, (3) tracking state to know "where to resume." CDC without idempotency can cause duplicate records or missed changes.

**Topic:** ETL/ELT & Data Pipelines

---

### Q75. A dbt (data build tool) model is defined as a `view`. The transformation logic is complex and runs slowly when queried. The business needs faster query performance. What dbt materialization change helps?

**A)** Change to `table` materialization for pre-computed results
**B)** Change to `incremental` materialization to only process new data
**C)** Both A and B are valid improvements; B is more efficient for large datasets
**D)** Use `ephemeral` materialization to push complexity to dependent models
**E)** The solution depends on how frequently the underlying data changes

**Answer:** D — Use `ephemeral` materialization to push complexity to dependent models

**Explanation:** Changing `view` to `table` pre-computes the model, improving query speed but adding maintenance overhead. `incremental` only processes new rows since last run, balancing speed and maintenance. The choice depends on data change frequency, model size, and how often it's queried. Note: dbt Core 1.6+ also supports `materialized view` as a fifth built-in materialization, which provides some of the benefits of both views and tables.

**Topic:** ETL/ELT & Data Pipelines

---

### Q76. In a pipeline that loads data from staging to production schema, some rows fail due to **foreign key violations**. The pipeline needs to continue processing other rows. Which pattern implements this correctly?

**A)** Insert all rows and let the database enforce constraints at the end
**B)** Use a **staging table** for raw data, then use `INSERT ... ON CONFLICT DO NOTHING` or filtered inserts
**C)** Disable foreign key constraints during load, re-enable after
**D)** A and C are both error-handling patterns used in production
**E)** B is the standard pattern; constraint violations should be logged and investigated

**Answer:** D — A and C are both error-handling patterns used in production

**Explanation:** Option A (insert all, let database fail) stops the transaction. Option C (disable constraints) risks data integrity. Option B uses a staging table to validate data before loading, with conflict handling (ignore duplicates, log errors) that allows partial success. This is the production-standard pattern for resilient data loading.

**Topic:** ETL/ELT & Data Pipelines

---

### Q77. A data engineer argues that **ELT** (load then transform) is always better than **ETL** (extract, transform, load). What's the most accurate position?

**A)** ELT is generally better for cloud data warehouses with powerful compute
**B)** All of the above are accurate statements
**C)** The choice depends on: target platform capabilities, data volume, transformation complexity, and security/compliance requirements
**D)** ETL is better when data needs to be cleaned before landing in the target system
**E)** ETL is obsolete in modern cloud data platforms

**Answer:** B — All of the above are accurate statements

**Explanation:** Cloud warehouses (Snowflake, BigQuery, Redshift) have powerful ELT engines that make ELT efficient. ETL makes sense when raw data shouldn't land in the target (PII filtering, sensitive data), when target has limited compute, or when transformations must happen before data movement for compliance. Neither is universally better.

**Topic:** ETL/ELT & Data Pipelines

---

### Q78. A pipeline task needs to **backfill** 5 years of historical data. The daily volume varies significantly (early years have less data). Which approach is most appropriate?

**A)** Run the same daily processing for each historical day sequentially
**B)** Use **partition-based backfill** to process larger time periods for recent data and smaller periods for older data
**C)** Parallelize backfill using task dependencies or external orchestration
**D)** B and C together allow efficient backfill across varying data volumes
**E)** Cloud data warehouse's time-travel or cloning features can accelerate backfills

**Answer:** D — B and C together allow efficient backfill across varying data volumes

**Explanation:** Backfill strategies should account for data volume variation and use platform capabilities. Processing 5 years sequentially is slow. Partition-based processing (larger windows for recent dense data) plus parallelization speeds up backfills. Modern cloud warehouses also provide time-travel/cloning features that can reduce redundant data reads. Note: Time Travel typically covers 1-90 days of history, not 5 years. For multi-year backfills, partition-based parallel processing is the primary strategy.

**Topic:** ETL/ELT & Data Pipelines

---

### Q79. In Airflow, you need to ensure that **T2 runs exactly once** even if T1 retries. T2 should not start until T1 succeeds, and if T1 runs twice, T2 should only trigger once. How do you configure this?

**A)** Use `unique` trigger mode to ensure tasks only fire once
**B)** Use `Airflow's built-in idempotency` features
**C)** Configure `wait_for_downstream=False` and use `latest_only` where appropriate
**D)** Use **DAG-level idempotency** by checking if work was already done before starting
**E)** Airflow guarantees at-least-once execution; exactly-once requires external state management

**Answer:** E — Airflow guarantees at-least-once execution; exactly-once requires external state management

**Explanation:** Airflow provides at-least-once execution guarantees, not exactly-once. If T1 retries, T2 may see multiple triggers. For exactly-once semantics, tasks must be idempotent (re-running produces same result) or use external state management (XCom with careful design, external locks, or deferrable operators). This is a fundamental architectural consideration.

**Topic:** ETL/ELT & Data Pipelines

---

### Q80. You need to implement **data lineage tracking** across a pipeline that spans multiple systems (Kafka → Spark → Snowflake → Tableau). What's the most practical approach?

**A)** Use OpenLineage standard for automatic lineage collection
**B)** Have each system emit lineage events to a central catalog
**C)** Document lineage manually in a data catalog
**D)** Use a metadata management platform that integrates with these tools
**E)** All of the above are used in practice; the choice depends on organization size and maturity

**Answer:** E — All of the above are used in practice; the choice depends on organization size and maturity

**Explanation:** OpenLineage is becoming the industry standard for automatic lineage. Manual documentation is error-prone but may be necessary for legacy systems. Commercial platforms (Collibra, Alation, Amundsen) provide integrated solutions. The appropriate approach depends on budget, team size, regulatory requirements, and existing tool stack.

**Topic:** ETL/ELT & Data Pipelines

---

### Q81. A dbt model has a SQL query that joins to a source table. The analyst updates the source table name in production. Your dbt model breaks. Which dbt feature helps manage source changes?

**A)** Using `ref()` instead of raw table names for model references
**B)** Using `source()` function for all source table references
**C)** Using dbt's `source freshness` check
**D)** All of the above help; `source()` is specifically for external/source table management
**E)** dbt doesn't provide source abstraction; use schema.yml for source definitions

**Answer:** D — All of the above help; `source()` is specifically for external/source table management

**Explanation:** The `source()` function in dbt reads from `schema.yml` which defines source tables. When sources change, updating the YAML file updates all models referencing that source. `ref()` is for model-to-model references. Using `source()` is the recommended practice for external table management.

**Topic:** ETL/ELT & Data Pipelines

---

### Q82. A nightly batch pipeline has a step that **archives** processed files by moving them to cold storage. The move sometimes fails mid-transfer. Which pattern ensures pipeline correctness?

**A)** Move the file, then update the tracking database; if move fails, no update occurs
**B)** Write metadata first, move file, then verify move succeeded before completing
**C)** Use a **two-phase commit** approach: prepare, then commit
**D)** Use **write-once, read-many (WORM)** storage that handles partial writes
**E)** B and C are both valid atomic operation patterns

**Answer:** E — B and C are both valid atomic operation patterns

**Explanation:** Option A (move then update) can leave orphaned state if move fails after partial success. Option B (write metadata, move, verify) ensures we can detect and retry failures. Option C (two-phase) works for distributed systems. Most archival solutions use a verification step (B) to confirm successful transfer before considering the operation complete.

**Topic:** ETL/ELT & Data Pipelines

---

## ADVANCED VISUALIZATION & STORYTELLING (8 Questions)

---

### Q83. A dashboard shows **monthly revenue trends** for 50 products. Most products have similar patterns, but 5 outliers have dramatic spikes. Which visualization approach best communicates both patterns?

**A)** Line chart showing all 50 products with different colors
**B)** **Small multiples**: one small chart per product, grouped by category, with outliers highlighted
**C)** Table with sparklines and outlier annotations
**D)** Heatmap with products on Y-axis and months on X-axis
**E)** B and D are both effective for high-cardinality categorical comparisons

**Answer:** E — B and D are both effective for high-cardinality categorical comparisons

**Explanation:** Showing 50 lines (A) creates visual chaos. Small multiples (B) allow pattern comparison across products while highlighting outliers. Heatmaps (D) efficiently show magnitude across two categorical dimensions. Both B and D are superior to over-plotting for high-cardinality data.

**Topic:** Advanced Visualization & Storytelling

---

### Q84. A stakeholder asks you to create a dashboard for **customer journey analysis** showing how users flow through stages: Visit → Signup → First Purchase → Repeat Purchase. Which chart type is most appropriate?

**A)** Standard bar chart comparing counts at each stage
**B)** **Sankey diagram** showing flow volumes between stages with attrition
**C)** Funnel chart showing conversion rates between stages
**D)** B or C depending on whether absolute volumes or conversion rates are more important
**E)** All of the above can work; the choice depends on the primary insight needed

**Answer:** E — All of the above can work; the choice depends on the primary insight needed

**Explanation:** Sankey diagrams show both volumes and flow paths (B). Funnel charts emphasize conversion rates (C). Bar charts show absolute counts (A). The choice depends on whether stakeholders care more about drop-off rates, absolute volumes, or understanding the flow paths. Multiple views may be needed.

**Topic:** Advanced Visualization & Storytelling

---

### Q85. You're presenting **quarterly results** to executives. The data shows strong growth but also a concerning trend in customer acquisition cost. How do you structure the presentation?

**A)** Show all metrics regardless of story coherence
**B)** Lead with the headline (growth), then address the concern, providing context and potential actions
**C)** Hide the concerning trend as it's not the main story
**D)** Show the data and let executives draw their own conclusions
**E)** B is the recommended approach; data storytelling guides what to reveal and when

**Answer:** B — Lead with the headline (growth), then address the concern, providing context and potential actions

**Explanation:** Effective data storytelling structures the narrative: headline → supporting evidence → tension (concern) → context → recommendations. Hiding inconvenient data is dishonest. Showing all data without narrative overwhelms. Executives appreciate recommendations alongside problems. The goal is to inform action, not just report numbers.

**Topic:** Advanced Visualization & Storytelling

---

### Q86. A heatmap shows **user activity by hour of day (X) and day of week (Y)**. The colors show intensity. Which additional element would make this visualization more accessible?

**A)** Pattern overlay (hatching) in addition to color for colorblind users
**B)** A diverging color palette centered at the median
**C)** Data labels showing exact values
**D)** A and B together address both accessibility and interpretation needs
**E)** All of A, B, and C are recommended for production dashboards

**Answer:** E — All of A, B, and C are recommended for production dashboards

**Explanation:** Pattern overlays (A) help colorblind users distinguish values. A diverging palette (B) makes deviations from median intuitive. Data labels (C) provide precise values without requiring color lookup. Production dashboards should consider accessibility standards (WCAG) to serve all users.

**Topic:** Advanced Visualization & Storytelling

---

### Q87. A dashboard requires showing **year-over-year comparison** with both absolute values and percentage change. Which design pattern is most effective?

**A)** Dual-axis chart with absolute values on left Y-axis and % change on right Y-axis
**B)** Bar chart with absolute values, with annotation labels showing % change
**C)** Bullet chart comparing current year to target/prior year
**D)** B is clear and simple; A's dual axis can be misleading if scales aren't comparable
**E)** B and C are both superior to dual-axis for most business dashboards

**Answer:** E — B and C are both superior to dual-axis for most business dashboards

**Explanation:** Dual-axis charts (A) are often misleading because the two Y-axes can have different scales, creating false visual relationships. Annotated bars (B) show both metrics without confusing scales. Bullet charts (C) are excellent for comparing actual vs target/prior. Simplicity and honesty in scale choices are paramount.

**Topic:** Advanced Visualization & Storytelling

---

### Q88. You're building a dashboard for **C-level executives** who have 2 minutes to review it. Which design principle is most critical?

**A)** Include all available data for completeness
**B)** Use the most complex visualizations to show analytical sophistication
**C)** **One primary insight** per dashboard, with supporting metrics below the fold
**D)** Use animation to draw attention to key metrics
**E)** C is correct; executive dashboards prioritize clarity and actionability over depth

**Answer:** E — C is correct; executive dashboards prioritize clarity and actionability over depth

**Explanation:** Executive dashboards prioritize quick comprehension (C). Every second counts; dashboards should surface one key insight with clear take-aways. All other design choices (animation, complexity, completeness) work against the primary goal of rapid insight. "One primary insight" doesn't mean hiding problems—it means leading with what matters most.

**Topic:** Advanced Visualization & Storytelling

---

### Q89. A visualization shows **correlation matrix** of 20 product attributes. The analyst asks for recommendations on improving the visualization. Which is most helpful?

**A)** Add more color gradients to show more nuance
**B)** Use **clustermap** (hierarchical clustering) to group similar attributes together
**C)** Add data labels with exact correlation values
**D)** Consider a scatter plot matrix for specific pairs of interest
**E)** B and D provide complementary views; clustermap for overview, scatter matrix for detail

**Answer:** E — B and D provide complementary views; clustermap for overview, scatter matrix for detail

**Explanation:** A 20×20 correlation matrix is hard to read. Clustermap (B) reorders rows/columns using hierarchical clustering, grouping highly correlated attributes visually. Scatter plot matrix (D) shows actual relationships for detailed analysis. Adding more colors or labels (A, C) increases clutter without improving pattern recognition.

**Topic:** Advanced Visualization & Storytelling

---

### Q90. You need to visualize **budget vs actual** for 50 departments. Some departments are over budget, some under. Which chart type communicates this best?

**A)** Stacked bar chart showing budget and actual side by side
**B)** **Bullet chart**: horizontal bars showing actual vs budget, with qualitative ranges
**C)** Waterfall chart showing variance breakdown
**D)** B is most effective for individual department comparison; C shows aggregate variance
**E)** All of the above can work depending on the primary question

**Answer:** E — All of the above can work depending on the primary question

**Explanation:** Bullet charts (B) are the standard for actual-vs-target comparisons, showing actual, target, and qualitative ranges (good/satisfactory/poor) in a compact space. Stacked bars (A) work for parts-to-whole but less intuitive for vs-target. Waterfall (C) is best for understanding how individual departments contribute to total variance. The choice depends on whether you're comparing to targets or analyzing variance composition.

**Topic:** Advanced Visualization & Storytelling

---

## CLOUD DATA PLATFORMS (10 Questions)

---

### Q91. Your analytics team needs to query **structured data in S3** (Parquet files, 10TB) alongside **live transactional data in RDS**. Queries frequently join these sources. Which AWS service combination is most appropriate?

**A)** Athena for S3 queries + RDS direct queries, then join in application
**B)** **Glue Data Catalog + Athena** for S3, with RDS connected via Glue connections, using Athena federated queries or copying RDS data to S3
**C)** EMR with Spark for all processing
**D)** Redshift Spectrum for querying S3 alongside Redshift tables
**E)** B or D depending on whether you need real-time RDS joins or periodic data movement is acceptable

**Answer:** E — B or D depending on whether you need real-time RDS joins or periodic data movement is acceptable

**Explanation:** Glue + Athena + Data Catalog (B) provides serverless S3 querying with catalog management. Redshift Spectrum (D) extends Redshift to query S3 without loading. For S3 + live RDS joins, either federated query or copying RDS to S3 (for Aurora → S3 export or DMS) is needed. Real-time joins between S3 Parquet and live RDS require careful architecture.

**Topic:** Cloud Data Platforms

---

### Q92. A data team uses **BigQuery** for analytics. They need to run ML models on their data. Which approach is most integrated?

**A)** Export data to Vertex AI for training
**B)** Use BigQuery **ML (BQML)** for in-database model training and prediction
**C)** Use Dataflow for feature engineering, then Vertex AI
**D)** B and C depending on model complexity; BQML for simpler models, Vertex for complex ones
**E)** All of the above are valid; the choice depends on ML requirements

**Answer:** E — All of the above are valid; the choice depends on ML requirements

**Explanation:** BQML provides convenience for standard ML (linear regression, recommendation systems) within BigQuery, with minimal data movement. Vertex AI provides full ML platform capabilities for complex models, custom frameworks, and MLOps. The choice depends on model complexity, team skills, and whether in-database processing is needed.

**Topic:** Cloud Data Platforms

---

### Q93. You're designing a data pipeline that reads from **Kafka topics**, processes streams, and writes aggregated results to a data warehouse. For **exactly-once processing**, which Azure service combination is most appropriate?

**A)** Event Hubs + Azure Functions + Synapse
**B)** **Kafka on HDInsight or Confluent Cloud + Spark Structured Streaming + destination with idempotent writes**
**C)** Event Hubs + Data Factory + Blob Storage
**D)** Kafka + Stream Analytics + SQL Database
**E)** B with idempotent sink writes is the correct pattern for exactly-once

**Answer:** E — B with idempotent sink writes is the correct pattern for exactly-once

**Explanation:** True exactly-once requires: (1) transactional source offsets committed only after successful processing, (2) idempotent destination writes. Kafka + Spark Structured Streaming (B) provides the processing guarantees. Writing to a sink with idempotent operations (upserts by key) ensures exactly-once semantics. Native Kafka connectors vary in exactly-once support.

**Topic:** Cloud Data Platforms

---

### Q94. A team migrates from an on-premise data warehouse to **Snowflake**. The analysts notice queries run faster but storage costs are higher than expected. What's the likely cause?

**A)** Snowflake's columnar storage compresses data efficiently
**B)** **Time travel and fail-safe features** consume additional storage for historical data
**C)** Data was denormalized during migration
**D)** Snowflake's micro-partitioning should reduce storage compared to traditional warehouses
**E)** B is correct; time travel storage is charged at standard rate

**Answer:** E — B is correct; time travel storage is charged at standard rate

**Explanation:** Snowflake's Time Travel (by default 1-90 days depending on edition) and Fail-safe storage consume additional storage that is billed at the same per-TB rate as active data, significantly increasing total storage costs. If analysts aren't aware of this, they may be surprised by storage costs. Proper data lifecycle management (purging old time travel, using transient tables for ETL) can control costs.

**Topic:** Cloud Data Platforms

---

### Q95. You need to create a **data lakehouse** architecture that supports both streaming (real-time) and batch (historical) workloads with ACID transactions. Which open-source stack is most appropriate?

**A)** HDFS + Hive + Spark
**B)** **Apache Iceberg or Delta Lake on object storage + Spark/Databricks**
**C)** S3 + Glue + Redshift Spectrum
**D)** Delta Lake only (open-source Apache 2.0 since v2.0, but specification controlled by Databricks)
**E)** B is the industry-standard open-source lakehouse approach

**Answer:** E — B is the industry-standard open-source lakehouse approach

**Explanation:** Apache Iceberg and Delta Lake provide ACID transactions, schema evolution, and time travel on top of object storage (S3, GCS, Azure Blob). They enable the "lakehouse" pattern—data lake economics with data warehouse reliability. Delta Lake is open-source under Apache 2.0 (since v2.0) but its specification is controlled by Databricks. Spark, Flink, or Trino can read/write these formats. The choice between Iceberg and Delta Lake depends on ecosystem and governance preferences.

**Topic:** Cloud Data Platforms

---

### Q96. An ETL pipeline needs to process **500GB of data daily** within a 4-hour window. The team uses AWS. Which compute service is most cost-effective for this workload?

**A)** Glue ETL with auto-scaling
**B)** EMR Serverless with auto-termination
**C)** Lambda for serverless processing
**D)** EC2 instances with auto-scaling group
**E)** Glue or EMR Serverless depending on job complexity; serverless removes cluster management overhead

**Answer:** E — Glue or EMR Serverless depending on job complexity; serverless removes cluster management overhead

**Explanation:** Glue Serverless (A) and EMR Serverless (B) both provide auto-scaling, pay-per-second pricing, and no cluster management. Glue is simpler for Python/Spark ETL; EMR provides more customization. Lambda (C) has 15-minute max execution time and is unsuitable for 500GB batch. EC2 (D) requires manual cluster management. Serverless is most cost-effective for variable, predictable batch workloads.

**Topic:** Cloud Data Platforms

---

### Q97. You need to **partition and cluster** a BigQuery table for optimal query performance. The most common query filters by `date` and aggregates by `region`. Which design is optimal?

**A)** Partition by `date`, cluster by `region`
**B)** Partition by `region`, cluster by `date`
**C)** Use window functions to pre-aggregate by region and date
**D)** Partition by date and use clustering for region; partition on the most commonly filtered temporal column
**E)** D is correct; BigQuery partitioning and clustering work together for filter and aggregation optimization

**Answer:** E — D is correct; BigQuery partitioning and clustering work together for filter and aggregation optimization

**Explanation:** Partition on the most commonly filtered high-cardinality column (date). Clustering on region groups data physically, improving aggregation performance. BigQuery allows one partitioning column (date/timestamp/integer) and up to 4 clustering columns. This co-design of partitioning + clustering optimizes both filter efficiency and aggregation speed.

**Topic:** Cloud Data Platforms

---

### Q98. A data analyst wants to run **ad-hoc queries** on S3 data (CSV, 50GB) without setting up infrastructure. Queries need to join with on-premise Oracle data. Which approach is most appropriate?

**A)** Use AWS Glue to create a Data Catalog and Athena for S3 queries, with Oracle connected via SQL Developer
**B)** Use **Athena with federated queries** or Lambda to join S3 data with Oracle
**C)** Load S3 data to RDS, then query
**D)** Use EMR for all processing
**E)** B is most appropriate for ad-hoc access; full ETL pipeline may need different architecture

**Answer:** E — B is most appropriate for ad-hoc access; full ETL pipeline may need different architecture

**Explanation:** Athena provides serverless ad-hoc querying on S3. For joining with on-premise Oracle, federated queries (Athena + Lambda connector) or loading Oracle data to S3 first (via DMS or export) are options. The key insight is that ad-hoc (Athena) and production pipelines (Glue/EMR/Data Factory) often have different optimal architectures.

**Topic:** Cloud Data Platforms

---

### Q99. You need to implement **row-level security (RLS)** in a cloud data warehouse for a multi-tenant SaaS application. Which statement is correct?

**A)** Snowflake: Use `CURRENT_ROLE()` and `SYSTEM$GET_USER_ENVIRONMENT()` in WHERE clauses
**B)** BigQuery: Use `ROW ACCESS POLICIES` or `DATA MASKING` with IAM conditions
**C)** Redshift: Use `CREATE POLICY` with role-based conditions
**D)** All cloud warehouses support RLS but implementation syntax differs
**E)** All of the above are correct; RLS implementation is vendor-specific

**Answer:** E — All of the above are correct; RLS implementation is vendor-specific

**Explanation:** All major cloud warehouses (Snowflake, BigQuery, Redshift) support RLS, but the implementation syntax and capabilities differ. Snowflake uses `CURRENT_ROLE()` and `SYSTEM$GET_USER_ENVIRONMENT()`. BigQuery uses ROW ACCESS POLICIES and column-level security. Redshift uses PostgreSQL-compatible `CREATE POLICY`. The security model must be designed per-vendor.

**Topic:** Cloud Data Platforms

---

### Q100. A team is choosing between **BigQuery slot reservations vs flat-rate pricing vs on-demand**. They have consistent daily workloads with 70% utilization of reserved capacity. Which pricing model is most cost-effective?

**A)** **BigQuery Editions (formerly flat-rate)** for consistent, predictable workloads; better than on-demand at this utilization
**B)** **On-demand** since 70% utilization suggests over-provisioning
**C)** **Slot reservations** for the 70% baseline, with on-demand for bursts
**D)** All can be analyzed via BigQuery pricing calculator; utilization isn't the only factor
**E)** C is most cost-effective; reserve for baseline, burst on-demand

**Answer:** E — C is most cost-effective; reserve for baseline, burst on-demand

**Explanation:** BigQuery slot reservations provide committed capacity at lower rates. On-demand is pay-per-query. For 70% consistent utilization, reserving slots for ~70% of peak and using on-demand for bursts is the hybrid approach many teams use. BigQuery Editions are essentially pre-purchased slots for large enterprises. The "best" depends on query patterns and commitment flexibility.

**Topic:** Cloud Data Platforms
