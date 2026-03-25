---

# Junior Data Analyst Engineer Assessment (0-2 Years Experience)

## 100 Multiple-Choice Questions

---

## SQL FUNDAMENTALS (25 Questions)

---

### Q1. Which SQL clause is used to filter rows before grouping?


**A)** HAVING
**B)** ORDER BY
**C)** GROUP BY
**D)** WHERE
**Answer:** D — WHERE
**Explanation:** The WHERE clause filters individual rows before any grouping occurs. HAVING filters groups after GROUP BY has been applied. This distinction is fundamental — WHERE operates on raw rows, HAVING operates on aggregated results.
**Topic:** SQL Fundamentals
---

### Q2. What does the following query return?

`SELECT COUNT(*) FROM employees WHERE department IS NULL;`


**A)** The total number of employees
**B)** The number of employees with no department assigned
**C)** An error because you cannot use IS NULL with COUNT
**D)** The number of distinct departments
**Answer:** B — The number of employees with no department assigned
**Explanation:** IS NULL checks for missing (NULL) values. COUNT(*) counts all rows that match the WHERE condition. This query counts employees where the department column has no value assigned. Remember, NULL represents the absence of a value, not zero or an empty string.
**Topic:** SQL Fundamentals

---

### Q3. Which JOIN type returns all rows from the left table and matched rows from the right table?



**A)** INNER JOIN
**B)** RIGHT JOIN
**C)** LEFT JOIN
**D)** CROSS JOIN
**Answer:** C — LEFT JOIN
**Explanation:** A LEFT JOIN (or LEFT OUTER JOIN) returns every row from the left table regardless of whether a match exists in the right table. If no match is found, NULL values are returned for the right table's columns. This is commonly used when you want to preserve all records from your primary table.
**Topic:** SQL Fundamentals

---

### Q4. What is the correct order of SQL clause execution?


**A)** SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY
**B)** SELECT → FROM → WHERE → ORDER BY → GROUP BY → HAVING
**C)** FROM → SELECT → WHERE → GROUP BY → HAVING → ORDER BY
**D)** FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
**Answer:** D — FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
**Explanation:** SQL's logical execution order differs from the written order. The database first identifies the table (FROM), filters rows (WHERE), groups them (GROUP BY), filters groups (HAVING), selects columns and computes expressions (SELECT), and finally sorts results (ORDER BY). Understanding this order helps debug queries and predict results.
**Topic:** SQL Fundamentals
---

### Q5. Which aggregate function ignores NULL values by default?



**A)** Only COUNT(*)
**B)** All aggregate functions except COUNT(*)
**C)** None of them — all include NULLs
**D)** Only SUM and AVG
**Answer:** B — All aggregate functions except COUNT(*)
**Explanation:** All aggregate functions (SUM, AVG, MIN, MAX, COUNT(column_name)) ignore NULL values. The one exception is COUNT(*), which counts all rows regardless of NULL values. COUNT(column_name) also ignores NULLs. This is why AVG can produce different results than SUM/COUNT when NULLs are present.
**Topic:** SQL Fundamentals

---

### Q6. You need to find duplicate email addresses in a users table. Which query is correct?



**A)** `SELECT email FROM users WHERE email = duplicate`
**B)** `SELECT email, COUNT(*) FROM users GROUP BY email HAVING COUNT(*) > 1`
**C)** `SELECT DISTINCT email FROM users WHERE COUNT(*) > 1`
**D)** `SELECT email FROM users ORDER BY COUNT(email)`
**Answer:** B — `SELECT email, COUNT(*) FROM users GROUP BY email HAVING COUNT(*) > 1`
**Explanation:** To find duplicates, you GROUP BY the column in question and use HAVING COUNT(*) > 1 to filter only groups with more than one occurrence. You cannot use WHERE with aggregate functions (that's what HAVING is for), and DISTINCT only removes duplicates from output without identifying them.
**Topic:** SQL Fundamentals

---

### Q7. What does `SELECT DISTINCT department, city FROM employees` return?



**A)** Unique departments only
**B)** Unique cities only
**C)** Unique combinations of department and city
**D)** An error because DISTINCT can only apply to one column
**Answer:** C — Unique combinations of department and city
**Explanation:** DISTINCT applies to the entire row of selected columns, not just the first column. So this query returns unique pairs of (department, city). A department can appear multiple times if paired with different cities. This is a common misconception — DISTINCT always operates on the full combination of selected columns.
**Topic:** SQL Fundamentals

---

### Q8. Which statement correctly adds a new column to an existing table?


**A)** `INSERT COLUMN age INT INTO employees`
**B)** `MODIFY TABLE employees ADD COLUMN age INT`
**C)** `UPDATE TABLE employees ADD age INT`
**D)** `ALTER TABLE employees ADD COLUMN age INT`
**Answer:** D — `ALTER TABLE employees ADD COLUMN age INT`
**Explanation:** ALTER TABLE is the DDL (Data Definition Language) command used to modify table structure. ADD COLUMN adds a new column to an existing table. INSERT is for adding rows (data), UPDATE is for modifying existing data, and MODIFY TABLE is not a valid SQL command.
**Topic:** SQL Fundamentals
---

### Q9. What is the result of `SELECT 5 + NULL`?



**A)** 5
**B)** 0
**C)** NULL
**D)** An error
**Answer:** C — NULL
**Explanation:** Any arithmetic operation involving NULL results in NULL. NULL represents an unknown value, and adding something to an unknown produces an unknown. This behavior extends to all operations: concatenation, comparison, and math. To handle NULLs, use COALESCE() (standard SQL) or database-specific alternatives like IFNULL() (MySQL) or ISNULL() (SQL Server) to provide default values.
**Topic:** SQL Fundamentals

---

### Q10. Which SQL statement is used to remove all rows from a table but keep the table structure?



**A)** DELETE FROM table_name
**B)** DROP TABLE table_name
**C)** TRUNCATE TABLE table_name
**D)** Both A and C
**Answer:** D — Both A and C
**Explanation:** Both DELETE FROM (without a WHERE clause) and TRUNCATE TABLE remove all rows while preserving the table structure. However, TRUNCATE is faster because it doesn't log individual row deletions and resets auto-increment counters. In PostgreSQL and SQL Server, TRUNCATE can be rolled back in a transaction; in MySQL and Oracle, it cannot. DROP TABLE removes the entire table including its structure.
**Topic:** SQL Fundamentals

---

### Q11. A `customers` table has 100 rows and an `orders` table has 50 rows. What does `SELECT * FROM customers CROSS JOIN orders` return?



**A)** 150 rows
**B)** 100 rows
**C)** 50 rows
**D)** 5000 rows
**Answer:** D — 5000 rows
**Explanation:** A CROSS JOIN produces a Cartesian product — every row from the first table is paired with every row from the second table. So 100 × 50 = 5,000 rows. Cross joins are rarely used intentionally but are important to understand because an accidental missing JOIN condition can produce one, leading to unexpectedly large result sets.
**Topic:** SQL Fundamentals

---

### Q12. Which function would you use to combine a first name and last name into a full name in SQL?


**A)** MERGE(first_name, last_name)
**B)** COMBINE(first_name, ' ', last_name)
**C)** JOIN(first_name, last_name)
**D)** CONCAT(first_name, ' ', last_name)
**Answer:** D — CONCAT(first_name, ' ', last_name)
**Explanation:** CONCAT() is supported by MySQL, PostgreSQL, and SQL Server for combining strings. In this case, we include a space character between the names. The || operator is another widely supported concatenation syntax (e.g., first_name || ' ' || last_name) and is the only option in Oracle, which does not support CONCAT(). MERGE, JOIN, and COMBINE are not string functions in SQL.
**Topic:** SQL Fundamentals
---

### Q13. What does the COALESCE function do?


**A)** Returns the first non-NULL value from a list of arguments
**B)** Converts data types
**C)** Combines two tables
**D)** Counts the number of NULL values
**Answer:** A — Returns the first non-NULL value from a list of arguments
**Explanation:** COALESCE takes multiple arguments and returns the first one that is not NULL. For example, COALESCE(phone_mobile, phone_home, phone_work, 'No phone') returns the first available phone number, or 'No phone' if all are NULL. It's invaluable for providing fallback values and handling missing data in queries.
**Topic:** SQL Fundamentals
---

### Q14. You have a sales table with columns: sale_date, amount, region. Which query calculates the running total of sales ordered by date?


**A)** `SELECT sale_date, amount, SUM(amount) OVER (ORDER BY sale_date) FROM sales`
**B)** `SELECT sale_date, SUM(amount) FROM sales GROUP BY sale_date`
**C)** `SELECT sale_date, RUNNING_TOTAL(amount) FROM sales`
**D)** `SELECT sale_date, amount + LAG(amount) OVER (ORDER BY sale_date) FROM sales`
**Answer:** A — `SELECT sale_date, amount, SUM(amount) OVER (ORDER BY sale_date) FROM sales`
**Explanation:** Window functions with SUM() OVER (ORDER BY ...) calculate a running (cumulative) total. Unlike GROUP BY, window functions don't collapse rows — you keep each individual row while computing the aggregate across a defined window. Note: the default frame is RANGE, meaning rows with the same date will share the same running total. Use ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW for strict row-by-row accumulation. This is one of the most practical window function use cases for data analysts.
**Topic:** SQL Fundamentals
---

### Q15. What is the difference between UNION and UNION ALL?


**A)** UNION ALL is faster but doesn't sort results
**B)** There is no difference
**C)** UNION ALL only works with two tables; UNION works with multiple
**D)** UNION removes duplicates; UNION ALL keeps all rows including duplicates
**Answer:** D — UNION removes duplicates; UNION ALL keeps all rows including duplicates
**Explanation:** UNION combines result sets and removes duplicate rows (like applying DISTINCT). UNION ALL combines result sets and keeps all rows, including duplicates. UNION ALL is faster because it doesn't need to perform the deduplication step. Use UNION ALL when you know there are no duplicates or when duplicates are acceptable.
**Topic:** SQL Fundamentals
---

### Q16. Which query correctly returns the second highest salary from an employees table?



**A)** `SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees)`
**B)** `SELECT salary FROM employees ORDER BY salary DESC LIMIT 2`
**C)** `SELECT SECOND(salary) FROM employees`
**D)** `SELECT MIN(salary) FROM employees`
**Answer:** A — `SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees)`
**Explanation:** This subquery approach first finds the maximum salary, then finds the maximum salary that is less than that — effectively the second highest distinct salary. Option B returns two rows (the top two salaries) rather than just the second highest. SECOND() is not a SQL function. For cases where multiple employees share the top salary and you need to account for ties, DENSE_RANK() OVER (ORDER BY salary DESC) is a more robust approach.
**Topic:** SQL Fundamentals

---

### Q17. In a database, what does a PRIMARY KEY constraint guarantee?


**A)** The column values are always numeric
**B)** The column values are encrypted
**C)** The column values are automatically indexed but can contain NULLs
**D)** The column values are unique and not NULL
**Answer:** D — The column values are unique and not NULL
**Explanation:** A PRIMARY KEY enforces two rules: uniqueness (no duplicate values) and NOT NULL (every row must have a value). Each table can have only one primary key, though it can span multiple columns (composite key). Primary keys also automatically create an index, which speeds up lookups and joins on that column.
**Topic:** SQL Fundamentals
---

### Q18. What will this query return?

`SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department HAVING avg_salary > 50000;`


**A)** All departments with their average salary
**B)** Departments where the average salary exceeds 50,000
**C)** An error in some databases because HAVING references an alias
**D)** Individual employees earning more than 50,000
**Answer:** C — An error in some databases because HAVING references an alias
**Explanation:** In many SQL databases (notably MySQL allows it, but standard SQL, PostgreSQL, and SQL Server do not), you cannot reference a column alias in the HAVING clause because HAVING is logically processed before SELECT assigns aliases. The correct syntax would be HAVING AVG(salary) > 50000. This is a common pitfall in SQL.
**Topic:** SQL Fundamentals

---

### Q19. Which type of subquery returns a single value that can be used with comparison operators?



**A)** Correlated subquery
**B)** Scalar subquery
**C)** Table subquery
**D)** Inline view
**Answer:** B — Scalar subquery
**Explanation:** A scalar subquery returns exactly one row and one column — a single value. This allows it to be used with comparison operators like =, >, <. For example: WHERE salary > (SELECT AVG(salary) FROM employees). If a scalar subquery returns more than one row, the database throws an error. Correlated subqueries re-execute for each row of the outer query but can return multiple values.
**Topic:** SQL Fundamentals

---

### Q20. You need to update the salary of all employees in the 'Sales' department to increase by 10%. Which query is correct?



**A)** `UPDATE employees SET salary = salary + 10% WHERE department = 'Sales'`
**B)** `UPDATE employees SET salary = salary * 1.10 WHERE department = 'Sales'`
**C)** `ALTER employees SET salary = salary * 1.10 WHERE department = 'Sales'`
**D)** `MODIFY employees SET salary = salary + 0.10 WHERE department = 'Sales'`
**Answer:** B — `UPDATE employees SET salary = salary * 1.10 WHERE department = 'Sales'`
**Explanation:** UPDATE is the DML command for modifying existing data. Multiplying by 1.10 correctly increases the salary by 10%. Option A uses invalid syntax (10% is not valid in SQL math). ALTER is for changing table structure, not data. Option D uses a non-existent MODIFY command and would only add 0.10 to the salary rather than a 10% increase.
**Topic:** SQL Fundamentals

---

### Q21. What does the CASE expression do in SQL?



**A)** Changes the data type of a column
**B)** Provides conditional logic similar to if-then-else
**C)** Switches between different tables
**D)** Creates a new index
**Answer:** B — Provides conditional logic similar to if-then-else
**Explanation:** The CASE expression allows conditional logic within SQL queries, acting like if-then-else statements. For example: CASE WHEN score >= 90 THEN 'A' WHEN score >= 80 THEN 'B' ELSE 'C' END. It's extremely useful for categorizing data, creating custom columns, and conditional aggregation without needing application-level code.
**Topic:** SQL Fundamentals

---

### Q22. Which statement about indexes is TRUE?



**A)** Indexes always speed up INSERT operations
**B)** Indexes speed up SELECT queries but can slow down INSERT, UPDATE, and DELETE
**C)** Every column should have an index for best performance
**D)** Indexes have no impact on storage
**Answer:** B — Indexes speed up SELECT queries but can slow down INSERT, UPDATE, and DELETE
**Explanation:** Indexes create additional data structures that speed up data retrieval (SELECT). However, every INSERT, UPDATE, or DELETE must also update these index structures, adding overhead to write operations. Indexes also consume storage space. The best practice is to index columns frequently used in WHERE, JOIN, and ORDER BY clauses, not every column.
**Topic:** SQL Fundamentals

---

### Q23. What is the purpose of a FOREIGN KEY constraint?


**A)** It ensures referential integrity between two tables
**B)** It encrypts the column data
**C)** It makes the column auto-increment
**D)** It prevents NULL values in the column
**Answer:** A — It ensures referential integrity between two tables
**Explanation:** A FOREIGN KEY creates a link between two tables by ensuring that values in the foreign key column must exist in the referenced table's primary key (or unique key). This maintains referential integrity — you can't have an order referencing a customer that doesn't exist. It prevents orphaned records and maintains data consistency.
**Topic:** SQL Fundamentals
---

### Q24. What does LAG() window function do?


**A)** Creates a delay in query execution
**B)** Calculates the time difference between two dates
**C)** Finds the lowest value in a partition
**D)** Accesses data from a previous row in the result set without a self-join
**Answer:** D — Accesses data from a previous row in the result set without a self-join
**Explanation:** LAG() is a window function that lets you access a value from a preceding row based on a specified ordering. For example, LAG(sales, 1) OVER (ORDER BY month) returns the previous month's sales alongside the current row. This is invaluable for comparing current values with previous ones — like calculating month-over-month growth — without needing a self-join.
**Topic:** SQL Fundamentals
---

### Q25. You have a query that runs slowly on a table with millions of rows. The query filters on a `created_date` column that has no index. What is the most likely first step to improve performance?



**A)** Rewrite the query using a subquery
**B)** Add an index on the `created_date` column
**C)** Use SELECT * instead of specific columns
**D)** Remove the WHERE clause
**Answer:** B — Add an index on the `created_date` column
**Explanation:** When a query filters on a column without an index, the database must scan every row (full table scan). Adding an index on created_date allows the database to quickly locate matching rows without scanning the entire table. This is usually the highest-impact, lowest-effort optimization. SELECT * actually hurts performance, and removing WHERE returns all rows.
**Topic:** SQL Fundamentals

---

## BASIC PYTHON FOR DATA ANALYSIS (15 Questions)

---

### Q26. Which Python library is primarily used for data manipulation and analysis with DataFrames?



**A)** NumPy
**B)** Matplotlib
**C)** pandas
**D)** scikit-learn
**Answer:** C — pandas
**Explanation:** pandas is the de facto standard library for data manipulation in Python. Its core data structures — DataFrame (2D table) and Series (1D array) — provide powerful tools for loading, cleaning, transforming, and analyzing data. NumPy handles numerical arrays, Matplotlib handles plotting, and scikit-learn handles machine learning.
**Topic:** Basic Python for Data Analysis

---

### Q27. What does `df.head()` return when called on a pandas DataFrame?


**A)** The last 5 rows
**B)** A summary of column statistics
**C)** The first 5 rows
**D)** The column names
**Answer:** C — The first 5 rows
**Explanation:** df.head() returns the first 5 rows of the DataFrame by default. You can pass a number to change this, e.g., df.head(10) for the first 10 rows. It's one of the first commands you use when exploring a new dataset to quickly see the structure and sample data. df.tail() returns the last rows.
**Topic:** Basic Python for Data Analysis
---

### Q28. How do you handle missing values by removing rows that contain any NaN in pandas?


**A)** `df.remove_null()`
**B)** `df.fillna(0)`
**C)** `df.dropna()`
**D)** `df.clean()`
**Answer:** C — `df.dropna()`
**Explanation:** df.dropna() removes rows containing any NaN (missing) values. You can customize it with parameters: axis=1 to drop columns instead, how='all' to only drop rows where all values are NaN, or subset=['col1'] to check specific columns. df.fillna(0) replaces NaN with 0 rather than removing rows. Always consider whether dropping or filling is more appropriate for your analysis.
**Topic:** Basic Python for Data Analysis
---

### Q29. What does the following code return?

```python
data = {'A': [1, 2, 3], 'B': [4, 5, 6]}
df = pd.DataFrame(data)
print(df.shape)
```

**A)** (2, 3)
**B)** (6,)
**C)** (3, 2)
**D)** 6
**Answer:** C — (3, 2)
**Explanation:** df.shape returns a tuple of (rows, columns). The dictionary has two keys ('A' and 'B'), creating 2 columns, and each key has a list of 3 values, creating 3 rows. So the shape is (3, 2). This is essential for quickly understanding the dimensions of your dataset and verifying that data loading produced the expected result.
**Topic:** Basic Python for Data Analysis
---

### Q30. Which method is used to group data and compute aggregate statistics in pandas?


**A)** `df.pivot()`
**B)** `df.sort_values()`
**C)** `df.merge()`
**D)** `df.groupby()`
**Answer:** D — `df.groupby()`
**Explanation:** df.groupby() splits data into groups based on one or more columns, then you apply aggregate functions like .sum(), .mean(), .count(). For example, df.groupby('department')['salary'].mean() computes the average salary per department. It's the pandas equivalent of SQL's GROUP BY and is one of the most frequently used operations in data analysis.
**Topic:** Basic Python for Data Analysis
---

### Q31. What is the output of the following code?

```python
my_list = [10, 20, 30, 40, 50]
print(my_list[1:4])
```

**A)** [10, 20, 30, 40]
**B)** [10, 20, 30]
**C)** [20, 30, 40, 50]
**D)** [20, 30, 40]
**Answer:** D — [20, 30, 40]
**Explanation:** Python slicing uses [start:stop] where start is inclusive and stop is exclusive. Index 1 is 20 (second element), and index 4 is exclusive, so it stops before 50. The result is elements at indices 1, 2, and 3: [20, 30, 40]. Remember: Python uses zero-based indexing, and slice endpoints are always exclusive.
**Topic:** Basic Python for Data Analysis
---

### Q32. How would you read a CSV file into a pandas DataFrame?


**A)** `pd.read_csv('file.csv')`
**B)** `pd.open_csv('file.csv')`
**C)** `pd.load_csv('file.csv')`
**D)** `pd.import_csv('file.csv')`
**Answer:** A — `pd.read_csv('file.csv')`
**Explanation:** pd.read_csv() is the standard function for loading CSV files into a DataFrame. It supports many parameters for handling different file formats: sep for custom delimiters, header for specifying header rows, encoding for character encoding, na_values for custom missing value indicators, and dtype for specifying column types.
**Topic:** Basic Python for Data Analysis
---

### Q33. What does `df['column'].value_counts()` return?


**A)** The number of unique values in the column
**B)** True if all values are unique, False otherwise
**C)** The sum of all values in the column
**D)** The frequency count of each unique value, sorted by frequency descending
**Answer:** D — The frequency count of each unique value, sorted by frequency descending
**Explanation:** value_counts() returns a Series containing the count of each unique value, sorted from most to least frequent. For example, if a 'color' column has ['red', 'blue', 'red', 'red', 'blue'], value_counts() returns red: 3, blue: 2. It's extremely useful for understanding the distribution of categorical data and spotting data quality issues.
**Topic:** Basic Python for Data Analysis
---

### Q34. Which of the following correctly creates a dictionary in Python?


**A)** `d = {'name': 'Alice', 'age': 25}`
**B)** `d = ['name': 'Alice', 'age': 25]`
**C)** `d = ('name': 'Alice', 'age': 25)`
**D)** `d = <'name': 'Alice', 'age': 25>`
**Answer:** A — `d = {'name': 'Alice', 'age': 25}`
**Explanation:** Python dictionaries use curly braces {} with key: value pairs. Square brackets [] create lists, parentheses () create tuples, and angle brackets are not used for data structures in Python. Dictionaries are fundamental in data work — they're used to create DataFrames, store configuration, and map values.
**Topic:** Basic Python for Data Analysis
---

### Q35. What does the `merge()` function do in pandas?



**A)** Appends rows from one DataFrame to another
**B)** Combines two DataFrames based on common columns (similar to SQL JOIN)
**C)** Splits a DataFrame into multiple smaller DataFrames
**D)** Removes duplicate columns from a DataFrame
**Answer:** B — Combines two DataFrames based on common columns (similar to SQL JOIN)
**Explanation:** pd.merge() combines two DataFrames based on shared columns, just like SQL JOINs. It supports inner, left, right, and outer joins via the 'how' parameter. For example, pd.merge(orders, customers, on='customer_id', how='left') is equivalent to a SQL LEFT JOIN. Use concat() for appending rows or columns.
**Topic:** Basic Python for Data Analysis

---

### Q36. What will the following code print?

```python
x = [1, 2, 3, 4, 5]
result = [i ** 2 for i in x if i > 2]
print(result)
```


**A)** [1, 4, 9, 16, 25]
**B)** [9, 16, 25]
**C)** [3, 4, 5]
**D)** [4, 9, 16, 25]
**Answer:** B — [9, 16, 25]
**Explanation:** This is a list comprehension with a conditional filter. It squares each element (i ** 2) but only for elements where i > 2. The elements greater than 2 are 3, 4, and 5, and their squares are 9, 16, and 25. List comprehensions are a Pythonic way to create lists and are commonly used in data transformation tasks.
**Topic:** Basic Python for Data Analysis

---

### Q37. How do you rename columns in a pandas DataFrame?



**A)** `df.columns.rename({'old': 'new'})`
**B)** `df.rename(columns={'old_name': 'new_name'})`
**C)** `df.set_columns({'old': 'new'})`
**D)** `df.change_name(columns={'old': 'new'})`
**Answer:** B — `df.rename(columns={'old_name': 'new_name'})`
**Explanation:** df.rename(columns={'old_name': 'new_name'}) renames specified columns using a dictionary mapping. You can rename multiple columns at once by adding more key-value pairs. Remember to use inplace=True or assign the result back to df, as rename() returns a new DataFrame by default. You can also directly assign a list to df.columns to rename all columns at once.
**Topic:** Basic Python for Data Analysis

---

### Q38. What is the difference between `.loc[]` and `.iloc[]` in pandas?



**A)** `.loc[]` uses integer positions; `.iloc[]` uses labels
**B)** `.loc[]` uses labels/names; `.iloc[]` uses integer positions
**C)** They are identical
**D)** `.loc[]` is for rows; `.iloc[]` is for columns
**Answer:** B — `.loc[]` uses labels/names; `.iloc[]` uses integer positions
**Explanation:** .loc[] accesses data using row/column labels (e.g., df.loc[0:5, 'name']), while .iloc[] uses integer-based positional indexing (e.g., df.iloc[0:5, 0]). A key difference: .loc[] is inclusive on both ends of a slice, while .iloc[] is exclusive on the end (like standard Python slicing). Mixing these up is a common source of bugs.
**Topic:** Basic Python for Data Analysis

---

### Q39. What does `df.describe()` provide?


**A)** The data types of all columns
**B)** The first and last rows of the DataFrame
**C)** Summary statistics (count, mean, std, min, max, percentiles) for numeric columns
**D)** A list of all column names
**Answer:** C — Summary statistics (count, mean, std, min, max, percentiles) for numeric columns
**Explanation:** df.describe() generates summary statistics for numeric columns including count, mean, standard deviation, min, 25th percentile, median (50th percentile), 75th percentile, and max. Use df.describe(include='all') to include non-numeric columns too. It's one of the first things to run during exploratory data analysis (EDA) to understand data distributions.
**Topic:** Basic Python for Data Analysis
---

### Q40. What does the `apply()` method do in pandas?



**A)** Applies a filter to remove rows
**B)** Applies a function along an axis of the DataFrame or to a Series
**C)** Applies a merge between two DataFrames
**D)** Applies a sort to the DataFrame
**Answer:** B — Applies a function along an axis of the DataFrame or to a Series
**Explanation:** apply() lets you run any function across rows or columns of a DataFrame, or on each element of a Series. For example, df['name'].apply(lambda x: x.upper()) uppercases all names, or df.apply(lambda row: row['price'] * row['quantity'], axis=1) computes row-wise totals. It's the go-to method when built-in pandas methods don't cover your transformation needs.
**Topic:** Basic Python for Data Analysis

---

## BASIC STATISTICS (15 Questions)

---

### Q41. What is the median of the following dataset: 3, 7, 8, 5, 12, 14, 21?



**A)** 8
**B)** 10
**C)** 7
**D)** 12
**Answer:** A — 8
**Explanation:** To find the median, first sort the data: 3, 5, 7, 8, 12, 14, 21. With 7 values (odd count), the median is the middle value — the 4th value, which is 8. The median is preferred over the mean when data has outliers because it's not affected by extreme values. If the count were even, you'd average the two middle values.
**Topic:** Basic Statistics

---

### Q42. A dataset has a mean of 50 and a standard deviation of 10. Assuming a normal distribution, approximately what percentage of data falls between 30 and 70?



**A)** 68%
**B)** 95%
**C)** 99.7%
**D)** 50%
**Answer:** B — 95%
**Explanation:** This is the empirical rule (68-95-99.7 rule). 30 and 70 are each 2 standard deviations from the mean (50 ± 20 = 50 ± 2×10). About 68% falls within 1 SD, 95% within 2 SDs, and 99.7% within 3 SDs. This rule provides quick estimates for normal distributions and is fundamental to understanding data spread.
**Topic:** Basic Statistics

---

### Q43. What type of chart is most appropriate for showing the distribution of a single continuous variable?


**A)** Histogram
**B)** Pie chart
**C)** Bar chart
**D)** Line chart
**Answer:** A — Histogram
**Explanation:** Histograms display the frequency distribution of a continuous variable by dividing the range into bins and showing counts. Unlike bar charts (which are for categorical data with gaps between bars), histograms have adjacent bars representing continuous ranges. They quickly reveal the shape, center, and spread of your data, including whether it's skewed or has outliers.
**Topic:** Basic Statistics
---

### Q44. What does a correlation coefficient of -0.85 indicate?



**A)** A strong positive relationship
**B)** A weak negative relationship
**C)** A strong negative relationship
**D)** No relationship
**Answer:** C — A strong negative relationship
**Explanation:** Correlation coefficients range from -1 to +1. The sign indicates direction (positive or negative), and the absolute value indicates strength. |0.85| = 0.85 is close to 1, indicating a strong relationship. The negative sign means as one variable increases, the other tends to decrease. Remember: correlation does not imply causation — two variables can be correlated without one causing the other.
**Topic:** Basic Statistics

---

### Q45. In a dataset, if the mean is significantly higher than the median, the distribution is most likely:



**A)** Normally distributed
**B)** Left-skewed (negatively skewed)
**C)** Right-skewed (positively skewed)
**D)** Uniform
**Answer:** C — Right-skewed (positively skewed)
**Explanation:** When the mean exceeds the median, the distribution is right-skewed (positively skewed), meaning there are extreme high values pulling the mean upward. Think of income distributions — most people earn moderate amounts (keeping the median lower), but a few very high earners pull the mean up. In left-skewed distributions, the mean is less than the median.
**Topic:** Basic Statistics

---

### Q46. What is a p-value in statistical hypothesis testing?



**A)** The probability that the null hypothesis is true
**B)** The probability of observing results at least as extreme as the data, assuming the null hypothesis is true
**C)** The probability that the alternative hypothesis is true
**D)** The probability of making a Type II error
**Answer:** B — The probability of observing results at least as extreme as the data, assuming the null hypothesis is true
**Explanation:** The p-value is the probability of obtaining results as extreme as (or more extreme than) what was observed, assuming the null hypothesis is true. A common threshold is 0.05 — if p < 0.05, we reject the null hypothesis. Importantly, the p-value is NOT the probability that the null hypothesis is true; this is a widespread misinterpretation.
**Topic:** Basic Statistics

---

### Q47. What measure of central tendency is most appropriate for highly skewed data with outliers?



**A)** Mean
**B)** Median
**C)** Mode
**D)** Standard deviation
**Answer:** B — Median
**Explanation:** The median is robust to outliers because it only depends on the middle position, not on actual values. The mean is pulled toward outliers, making it misleading for skewed data. For example, if five employees earn $40K-$60K and one CEO earns $5M, the mean salary (~$900K) misrepresents the typical employee, while the median (~$50K) accurately reflects the center.
**Topic:** Basic Statistics

---

### Q48. What is the mode of this dataset: 4, 7, 7, 9, 12, 7, 15?



**A)** 7
**B)** 9
**C)** 8.71
**D)** 12
**Answer:** A — 7
**Explanation:** The mode is the value that appears most frequently in a dataset. Here, 7 appears three times while all other values appear once, making 7 the mode. A dataset can be unimodal (one mode), bimodal (two modes), or multimodal. The mode is the only measure of central tendency applicable to categorical data (e.g., the most common color).
**Topic:** Basic Statistics

---

### Q49. If you flip a fair coin 3 times, what is the probability of getting exactly 2 heads?


**A)** 1/8
**B)** 1/2
**C)** 3/8
**D)** 1/4
**Answer:** C — 3/8
**Explanation:** This is a binomial probability problem. The possible outcomes for 3 flips are 2³ = 8. The combinations with exactly 2 heads are: HHT, HTH, THH — that's 3 combinations. So the probability is 3/8. You can also use the formula: C(3,2) × (0.5)² × (0.5)¹ = 3 × 0.25 × 0.5 = 0.375 = 3/8.
**Topic:** Basic Statistics
---

### Q50. What does a box plot (box-and-whisker plot) display?


**A)** Only the mean and standard deviation
**B)** The correlation between two variables
**C)** The frequency of each category
**D)** The five-number summary: minimum, Q1, median, Q3, maximum
**Answer:** D — The five-number summary: minimum, Q1, median, Q3, maximum
**Explanation:** A box plot visualizes the five-number summary: minimum, first quartile (Q1), median (Q2), third quartile (Q3), and maximum. The box spans from Q1 to Q3 (the interquartile range), with a line at the median. Whiskers extend to the min and max (or to 1.5×IQR, with outliers shown as individual points). Box plots are excellent for comparing distributions across groups.
**Topic:** Basic Statistics
---

### Q51. What is a Type I error in hypothesis testing?



**A)** Failing to reject a false null hypothesis
**B)** Rejecting a true null hypothesis (false positive)
**C)** Accepting the alternative hypothesis correctly
**D)** Using the wrong statistical test
**Answer:** B — Rejecting a true null hypothesis (false positive)
**Explanation:** A Type I error (false positive) occurs when you reject the null hypothesis even though it's actually true — you conclude there's an effect when there isn't one. The probability of a Type I error is the significance level (α), typically set at 0.05. A Type II error (false negative) is failing to reject a false null hypothesis — missing a real effect. Both errors carry real-world consequences.
**Topic:** Basic Statistics

---

### Q52. Which of the following is a measure of data spread/variability?



**A)** Mean
**B)** Median
**C)** Variance
**D)** Mode
**Answer:** C — Variance
**Explanation:** Variance measures how far data points are spread from the mean. It's calculated as the average of squared deviations from the mean. Standard deviation (the square root of variance) is more commonly used because it's in the same units as the data. Mean, median, and mode are measures of central tendency, not spread. Other spread measures include range and IQR.
**Topic:** Basic Statistics

---

### Q53. A company surveys 200 customers out of 50,000 total. The 200 customers represent a:



**A)** Population
**B)** Parameter
**C)** Sample
**D)** Census
**Answer:** C — Sample
**Explanation:** A sample is a subset of a population selected for study. The 200 customers are a sample drawn from the population of 50,000. Statistics computed from a sample (like sample mean) are called statistics, while values describing the entire population are called parameters. A census surveys every member of the population. Good sampling techniques ensure the sample is representative.
**Topic:** Basic Statistics

---

### Q54. What does the interquartile range (IQR) represent?


**A)** The range of the entire dataset
**B)** The average distance from the mean
**C)** The difference between the mean and median
**D)** The range of the middle 50% of the data (Q3 - Q1)
**Answer:** D — The range of the middle 50% of the data (Q3 - Q1)
**Explanation:** The IQR is calculated as Q3 minus Q1, capturing the spread of the central 50% of the data. It's robust to outliers, unlike the full range (max - min). IQR is commonly used to detect outliers: values below Q1 - 1.5×IQR or above Q3 + 1.5×IQR are considered outliers. This is exactly what box plots use for their whisker boundaries.
**Topic:** Basic Statistics
---

### Q55. If two events are independent, what is the probability of both occurring?


**A)** P(A) × P(B)
**B)** P(A) + P(B)
**C)** P(A) - P(B)
**D)** P(A) / P(B)
**Answer:** A — P(A) × P(B)
**Explanation:** For independent events (where the occurrence of one doesn't affect the other), the probability of both occurring is the product of their individual probabilities: P(A and B) = P(A) × P(B). For example, the probability of rolling a 6 on one die (1/6) AND flipping heads (1/2) is 1/6 × 1/2 = 1/12. Addition (P(A) + P(B)) applies to mutually exclusive "or" scenarios.
**Topic:** Basic Statistics
---

## DATA MODELING BASICS (10 Questions)

---

### Q56. What is a fact table in a star schema?



**A)** A table that stores descriptive attributes about business entities
**B)** A table that contains measurable, quantitative data (metrics) for business events
**C)** A table that stores only primary keys
**D)** A temporary table used during data loading
**Answer:** B — A table that contains measurable, quantitative data (metrics) for business events
**Explanation:** A fact table sits at the center of a star schema and contains quantitative measurements (facts) like sales amount, quantity, and revenue, along with foreign keys linking to dimension tables. Each row typically represents a business event or transaction. Fact tables tend to be large (millions/billions of rows) and narrow (few columns relative to dimensions).
**Topic:** Data Modeling Basics

---

### Q57. What is the purpose of a dimension table?


**A)** To store transaction-level numeric data
**B)** To aggregate data for faster queries
**C)** To provide descriptive context (who, what, where, when) for facts
**D)** To store temporary staging data
**Answer:** C — To provide descriptive context (who, what, where, when) for facts
**Explanation:** Dimension tables provide the descriptive attributes that give context to the numbers in fact tables. A date dimension might include year, quarter, month, day name, and holiday flag. A product dimension might include name, category, brand, and price. They answer the "who, what, where, when, why, and how" of your business events. Dimensions are typically smaller and wider than fact tables.
**Topic:** Data Modeling Basics
---

### Q58. What does database normalization primarily aim to achieve?


**A)** Improve query speed by duplicating data
**B)** Store all data in a single table for simplicity
**C)** Create more tables to increase complexity
**D)** Reduce data redundancy and prevent update anomalies
**Answer:** D — Reduce data redundancy and prevent update anomalies
**Explanation:** Normalization organizes database tables to minimize data redundancy and dependency. By decomposing tables into smaller, related tables, you prevent anomalies: update anomalies (changing data in one place but not another), insertion anomalies (can't add data without unrelated data), and deletion anomalies (losing data unintentionally when deleting). The trade-off is more complex queries requiring JOINs.
**Topic:** Data Modeling Basics
---

### Q59. What is Third Normal Form (3NF)?



**A)** A table where every column depends only on the primary key, and there are no transitive dependencies
**B)** A table with no duplicate rows
**C)** A table with at least three columns
**D)** A table that has been partitioned into three sections
**Answer:** A — A table where every column depends only on the primary key, and there are no transitive dependencies
**Explanation:** 3NF requires that a table is already in 2NF and that every non-key column depends directly on the primary key — not on another non-key column (no transitive dependencies). For example, if an employee table has department_id AND department_name, the department_name depends on department_id, not the employee's primary key. The fix: move department_name to a separate department table.
**Topic:** Data Modeling Basics

---

### Q60. What is the difference between a star schema and a snowflake schema?



**A)** Star schemas are for small databases; snowflake schemas are for large ones
**B)** In a star schema, dimensions are denormalized; in a snowflake schema, dimensions are normalized into sub-tables
**C)** They are the same thing with different names
**D)** Star schemas only support numeric data; snowflake schemas support all types
**Answer:** B — In a star schema, dimensions are denormalized; in a snowflake schema, dimensions are normalized into sub-tables
**Explanation:** In a star schema, dimension tables are denormalized (flat) — all attributes are in one table. In a snowflake schema, dimensions are further normalized into sub-tables (e.g., a product dimension splits into product, category, and brand tables). Star schemas are simpler and faster to query. Snowflake schemas reduce storage and redundancy but require more JOINs. Star schemas are more common in data warehouses.
**Topic:** Data Modeling Basics

---

### Q61. What is a surrogate key?



**A)** A key derived from business data like email or SSN
**B)** An artificially generated key (like an auto-increment integer) with no business meaning
**C)** A foreign key that references another table
**D)** A key that changes over time
**Answer:** B — An artificially generated key (like an auto-increment integer) with no business meaning
**Explanation:** A surrogate key is a system-generated identifier (typically auto-incrementing integer or UUID) that uniquely identifies each row but has no business meaning. Unlike natural keys (email, SSN, product code), surrogate keys never change, are compact, and aren't affected by business rule changes. They're standard practice in data warehouses for dimension tables to handle slowly changing dimensions.
**Topic:** Data Modeling Basics

---

### Q62. Which type of Slowly Changing Dimension (SCD) overwrites old data with new data, keeping no history?


**A)** SCD Type 0
**B)** SCD Type 3
**C)** SCD Type 2
**D)** SCD Type 1
**Answer:** D — SCD Type 1
**Explanation:** SCD Type 1 simply overwrites the old value with the new one — no history is kept. For example, if a customer changes address, the old address is replaced. SCD Type 2 maintains full history by adding new rows with effective dates. SCD Type 3 keeps limited history (usually one previous value) by adding a column. Type 1 is simplest but means you lose the ability to analyze historical states.
**Topic:** Data Modeling Basics
---

### Q63. What is an Entity-Relationship (ER) diagram used for?


**A)** Visually representing entities, their attributes, and relationships in a database
**B)** Showing the flow of data through a system
**C)** Displaying query execution plans
**D)** Monitoring database performance
**Answer:** A — Visually representing entities, their attributes, and relationships in a database
**Explanation:** ER diagrams are a visual blueprint for database design. They show entities (tables), their attributes (columns), and relationships between entities (one-to-one, one-to-many, many-to-many). They're created during the design phase to communicate database structure to stakeholders and developers before implementation. They're essential for planning and documenting data models.
**Topic:** Data Modeling Basics
---

### Q64. In a one-to-many relationship between `customers` and `orders`, where should the foreign key be placed?


**A)** In the orders table
**B)** In the customers table
**C)** In a separate junction table
**D)** In both tables
**Answer:** A — In the orders table
**Explanation:** The foreign key goes on the "many" side of the relationship. Since one customer can have many orders, the orders table gets a customer_id foreign key. This way, each order points to its customer, and one customer can be referenced by multiple orders. A junction (bridge) table is used for many-to-many relationships, not one-to-many.
**Topic:** Data Modeling Basics
---

### Q65. What is denormalization, and when is it used?


**A)** The process of removing all redundancy from a database
**B)** Deleting unused tables from a database
**C)** Converting all data to text format
**D)** Intentionally adding redundancy to improve read performance at the cost of write complexity
**Answer:** D — Intentionally adding redundancy to improve read performance at the cost of write complexity
**Explanation:** Denormalization deliberately introduces data redundancy — the opposite of normalization — to reduce the number of JOINs needed for queries, improving read performance. It's commonly used in data warehouses and reporting databases where read performance is prioritized over write efficiency. The trade-off is increased storage and more complex data maintenance (keeping redundant copies in sync).
**Topic:** Data Modeling Basics
---

## DATA VISUALIZATION (10 Questions)

---

### Q66. Which chart type is best for showing trends over time?


**A)** Line chart
**B)** Pie chart
**C)** Scatter plot
**D)** Treemap
**Answer:** A — Line chart
**Explanation:** Line charts excel at showing how values change over continuous time periods. The x-axis represents time, and the connected points make trends, patterns, and anomalies easy to spot. They're ideal for time series data like monthly revenue, daily active users, or stock prices. Multiple lines can show comparisons between categories over the same time period.
**Topic:** Data Visualization
---

### Q67. When should you use a pie chart?



**A)** To show changes over time
**B)** To compare exact values between many categories
**C)** To show parts of a whole when there are few categories (ideally ≤5)
**D)** To display the distribution of a continuous variable
**Answer:** C — To show parts of a whole when there are few categories (ideally ≤5)
**Explanation:** Pie charts work best for showing proportions of a whole when there are few categories (ideally 2-5). Humans struggle to compare angles accurately, so pie charts become ineffective with many slices or similar-sized portions. For more categories or precise comparisons, use a horizontal bar chart instead. Despite their popularity, many data visualization experts recommend avoiding pie charts altogether.
**Topic:** Data Visualization

---

### Q68. What is the primary purpose of a scatter plot?



**A)** To show the relationship between two continuous variables
**B)** To display frequency distributions
**C)** To compare categories
**D)** To show proportions of a whole
**Answer:** A — To show the relationship between two continuous variables
**Explanation:** Scatter plots display the relationship between two continuous variables by plotting individual data points on an x-y coordinate plane. They reveal patterns like linear/nonlinear relationships, clusters, and outliers. Adding a trend line helps quantify the relationship. They're essential for exploratory analysis — for example, plotting marketing spend vs. revenue to assess if increased spending drives sales.
**Topic:** Data Visualization

---

### Q69. What does a heat map effectively communicate?


**A)** The magnitude of values across two dimensions using color intensity
**B)** Trends over time
**C)** Parts of a whole
**D)** Individual data points
**Answer:** A — The magnitude of values across two dimensions using color intensity
**Explanation:** Heat maps use color gradients to represent values in a matrix format, making it easy to spot patterns, concentrations, and outliers across two categorical dimensions. Common uses include correlation matrices, website click maps, geographic data, and time-based patterns (like sales by hour and day of week). The human eye is naturally drawn to color differences, making patterns immediately apparent.
**Topic:** Data Visualization
---

### Q70. Which of the following is a best practice in data visualization?



**A)** Use as many colors as possible to make charts visually interesting
**B)** Always include 3D effects for a professional appearance
**C)** Label axes clearly and provide a descriptive title
**D)** Truncate the y-axis starting at a high number to emphasize differences
**Answer:** C — Label axes clearly and provide a descriptive title
**Explanation:** Clear axis labels and descriptive titles are fundamental to good visualization. Too many colors create confusion rather than clarity. 3D effects add visual noise and can distort perception of values. Truncating the y-axis (not starting at zero for bar charts) can mislead readers by exaggerating small differences. The goal of visualization is to communicate data honestly and clearly.
**Topic:** Data Visualization

---

### Q71. You need to compare the sales performance of 15 different products. Which chart is most appropriate?


**A)** Horizontal bar chart
**B)** Pie chart
**C)** Line chart
**D)** Scatter plot
**Answer:** A — Horizontal bar chart
**Explanation:** Horizontal bar charts are ideal for comparing values across many categories. They accommodate long category names without rotation, make length comparisons easy (humans perceive lengths more accurately than angles), and scale well to 15+ categories. Pie charts fail with this many categories. Line charts are for time series. Scatter plots are for two continuous variables.
**Topic:** Data Visualization
---

### Q72. What is a dashboard in data visualization?


**A)** A collection of visualizations and metrics organized to provide an at-a-glance view of key information
**B)** A single chart showing all data
**C)** A database management tool
**D)** A report exported to PDF
**Answer:** A — A collection of visualizations and metrics organized to provide an at-a-glance view of key information
**Explanation:** A dashboard combines multiple visualizations, KPIs, and metrics into a single interface that provides a comprehensive overview of important data at a glance. Good dashboards are designed for specific audiences, tell a story, enable drill-down into details, and update automatically. They're the primary deliverable for data analysts communicating with business stakeholders.
**Topic:** Data Visualization
---

### Q73. When visualizing data, why is it important to start the y-axis at zero for bar charts?



**A)** It's only an aesthetic preference
**B)** Starting at non-zero values can visually exaggerate differences and mislead viewers
**C)** The chart software requires it
**D)** It makes the chart load faster
**Answer:** B — Starting at non-zero values can visually exaggerate differences and mislead viewers
**Explanation:** For bar charts, not starting at zero distorts the visual proportions. A bar chart where the y-axis starts at 95 instead of 0 might make a 97 vs. 100 difference look enormous when it's only a 3% difference. This is a common technique in misleading visualizations. Note: line charts can start at non-zero values since they show trends, not absolute comparisons.
**Topic:** Data Visualization

---

### Q74. What is the purpose of a KPI (Key Performance Indicator) in a dashboard?


**A)** To display raw data tables
**B)** To list all available data sources
**C)** To show the database schema
**D)** To highlight a specific, measurable metric that tracks progress toward a business goal
**Answer:** D — To highlight a specific, measurable metric that tracks progress toward a business goal
**Explanation:** KPIs are specific, quantifiable metrics that measure performance against business objectives. Examples include monthly recurring revenue, customer churn rate, average response time, or conversion rate. On dashboards, KPIs are typically displayed as prominent single numbers or gauges, often with trend indicators (up/down arrows) and comparison to targets or previous periods.
**Topic:** Data Visualization
---

### Q75. Which visualization tool allows you to create interactive dashboards with drag-and-drop functionality?


**A)** Tableau
**B)** Microsoft Notepad
**C)** Git
**D)** SQL Server Management Studio
**Answer:** A — Tableau
**Explanation:** Tableau is a leading data visualization tool known for its intuitive drag-and-drop interface for creating interactive dashboards. Other popular tools include Power BI (Microsoft), Looker (Google), and open-source options like Apache Superset. These tools connect to various data sources and enable non-technical users to explore data through visual interfaces without writing code.
**Topic:** Data Visualization
---

## EXCEL/SPREADSHEET SKILLS (10 Questions)

---

### Q76. What does the VLOOKUP function do in Excel?


**A)** Counts the number of cells that match a criteria
**B)** Calculates the sum of a range based on conditions
**C)** Searches for a value in the first column of a range and returns a value from a specified column
**D)** Sorts data in ascending order
**Answer:** C — Searches for a value in the first column of a range and returns a value from a specified column
**Explanation:** VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup]) searches the first column of a table for a match and returns a value from the specified column. For example, VLOOKUP("A123", products, 3, FALSE) finds "A123" in the first column and returns the value from the 3rd column. FALSE means exact match. Modern Excel favors XLOOKUP, which is more flexible.
**Topic:** Excel/Spreadsheet Skills
---

### Q77. What is the difference between a relative reference (A1) and an absolute reference ($A$1) in Excel?


**A)** There is no difference
**B)** Absolute references are faster to calculate
**C)** Relative references change when copied to other cells; absolute references remain fixed
**D)** Relative references only work with numbers
**Answer:** C — Relative references change when copied to other cells; absolute references remain fixed
**Explanation:** When you copy a formula, relative references (A1) adjust based on the new position — moving right changes A1 to B1. Absolute references ($A$1) stay fixed regardless of where the formula is copied. You can also mix them: $A1 (column fixed, row adjusts) or A$1 (row fixed, column adjusts). Press F4 to cycle through reference types. This is essential for building correct spreadsheet formulas.
**Topic:** Excel/Spreadsheet Skills
---

### Q78. Which Excel function would you use to count cells that meet multiple criteria?



**A)** COUNT
**B)** COUNTIF
**C)** COUNTIFS
**D)** SUMPRODUCT
**Answer:** C — COUNTIFS
**Explanation:** COUNTIFS allows multiple criteria across multiple ranges. For example, COUNTIFS(A:A, "Sales", B:B, ">50000") counts rows where column A is "Sales" AND column B exceeds 50,000. COUNTIF only supports a single criterion. COUNT counts numeric cells without criteria. SUMPRODUCT can also handle multiple criteria but is more complex and primarily designed for multiplication-then-summation.
**Topic:** Excel/Spreadsheet Skills

---

### Q79. What is a pivot table used for?


**A)** Formatting cells with colors and borders
**B)** Importing data from external sources
**C)** Creating charts and graphs
**D)** Summarizing, analyzing, and exploring large datasets by grouping and aggregating data
**Answer:** D — Summarizing, analyzing, and exploring large datasets by grouping and aggregating data
**Explanation:** Pivot tables are one of Excel's most powerful features for data analysis. They let you drag and drop fields to instantly summarize large datasets — grouping by categories, computing aggregates (sum, count, average), and cross-tabulating dimensions. For example, you can quickly see total sales by region and product category. They're the spreadsheet equivalent of SQL's GROUP BY with multiple dimensions.
**Topic:** Excel/Spreadsheet Skills
---

### Q80. What does the IF function return when the condition is FALSE and no false_value is provided?

`=IF(A1>10, "High")`


**A)** An error
**B)** 0
**C)** FALSE
**D)** An empty string ""
**Answer:** C — FALSE
**Explanation:** When the false_value argument is omitted in an IF function and the condition evaluates to FALSE, Excel returns the logical value FALSE. If you want a blank cell instead, explicitly provide an empty string: =IF(A1>10, "High", ""). This is a subtle behavior that can cause issues in downstream calculations if not handled properly.
**Topic:** Excel/Spreadsheet Skills

---

### Q81. What does the CONCATENATE function (or the & operator) do?


**A)** Joins two or more text strings into one
**B)** Adds numerical values together
**C)** Splits text into multiple cells
**D)** Finds and replaces text
**Answer:** A — Joins two or more text strings into one
**Explanation:** CONCATENATE (or the modern CONCAT function, or the & operator) joins text strings together. For example, =A1 & " " & B1 combines first and last names with a space. The & operator is generally preferred for its simplicity: ="Hello" & " " & "World" produces "Hello World". For joining with delimiters, the newer TEXTJOIN function is even more convenient.
**Topic:** Excel/Spreadsheet Skills
---

### Q82. Which function removes leading and trailing spaces from text in Excel?


**A)** TRIM
**B)** CLEAN
**C)** STRIP
**D)** CUT
**Answer:** A — TRIM
**Explanation:** TRIM removes all leading and trailing spaces from text, and reduces multiple internal spaces to a single space. This is essential for data cleaning — extra spaces are invisible but cause VLOOKUP failures, incorrect matches, and duplicate detection issues. CLEAN removes non-printable characters but not spaces. STRIP and CUT are not Excel functions.
**Topic:** Excel/Spreadsheet Skills
---

### Q83. What is conditional formatting in Excel?


**A)** Formatting that applies only when printing
**B)** Formatting that requires a password to view
**C)** Rules that automatically change cell appearance (color, font, icons) based on cell values
**D)** A way to hide columns based on conditions
**Answer:** C — Rules that automatically change cell appearance (color, font, icons) based on cell values
**Explanation:** Conditional formatting dynamically changes the visual appearance of cells based on their values or formulas. Common uses include: highlighting cells above a threshold, color-coding sales performance (red/yellow/green), showing data bars for quick comparison, and flagging duplicates. It makes patterns and outliers immediately visible without changing the data itself.
**Topic:** Excel/Spreadsheet Skills
---

### Q84. What does the INDEX-MATCH combination do that VLOOKUP cannot?


**A)** It runs faster on small datasets
**B)** It automatically sorts data
**C)** It can only work with numerical data
**D)** It can look up values to the left of the search column
**Answer:** D — It can look up values to the left of the search column
**Explanation:** VLOOKUP can only search the leftmost column and return values to the right. INDEX-MATCH has no such limitation — it can look in any direction. INDEX(return_range, MATCH(lookup_value, lookup_range, 0)) finds the position with MATCH and retrieves the value with INDEX. It's also more robust because inserting/deleting columns won't break it (unlike VLOOKUP's column index number).
**Topic:** Excel/Spreadsheet Skills
---

### Q85. You receive a CSV file with dates formatted as text (e.g., "2024-01-15"). What is the best way to convert these to proper Excel date values?


**A)** Manually retype each date
**B)** Use the SUM function
**C)** Use VLOOKUP to find the dates
**D)** Use the DATEVALUE function to convert the text string to a date serial number
**Answer:** D — Use the DATEVALUE function to convert the text string to a date serial number
**Explanation:** DATEVALUE() converts a text string that looks like a date into an Excel date serial number, enabling date calculations and formatting. For example, =DATEVALUE("2024-01-15") returns the serial number 45306, which you can then format as any date format. This is a common data cleaning task when importing CSV files where dates are stored as text strings.
**Topic:** Excel/Spreadsheet Skills
---

## BASIC ETL CONCEPTS (8 Questions)

---

### Q86. What does ETL stand for?



**A)** Extract, Transform, Load
**B)** Enter, Test, Launch
**C)** Export, Transfer, Log
**D)** Evaluate, Track, List
**Answer:** A — Extract, Transform, Load
**Explanation:** ETL stands for Extract, Transform, Load — the three phases of moving data from source systems to a target system (typically a data warehouse). Extract pulls data from sources, Transform cleans and reshapes it (handling nulls, converting types, applying business rules), and Load writes it to the destination. ETL is the backbone of data warehousing and analytics pipelines.
**Topic:** Basic ETL Concepts

---

### Q87. During the "Transform" phase of ETL, which of the following activities is commonly performed?


**A)** Connecting to source databases
**B)** Writing data to the target database
**C)** Data cleaning, type conversion, deduplication, and applying business rules
**D)** Creating user dashboards
**Answer:** C — Data cleaning, type conversion, deduplication, and applying business rules
**Explanation:** The Transform phase is where data is cleaned and prepared: handling missing values, converting data types, standardizing formats (e.g., date formats), deduplicating records, applying business logic (e.g., calculating derived metrics), and joining data from multiple sources. This is typically the most complex and time-consuming phase of ETL, and where data quality is ensured.
**Topic:** Basic ETL Concepts
---

### Q88. What is the difference between a full load and an incremental load?


**A)** Full load is faster than incremental load
**B)** Incremental load deletes old data; full load keeps everything
**C)** Full load replaces all data; incremental load adds only new or changed data since the last load
**D)** There is no difference
**Answer:** C — Full load replaces all data; incremental load adds only new or changed data since the last load
**Explanation:** A full load extracts and loads the complete dataset every time, replacing existing data. An incremental load only processes records that are new or modified since the last load. Incremental loads are much more efficient for large datasets — processing 1,000 new orders is faster than reloading 10 million historical orders. However, they're more complex to implement, requiring change detection mechanisms.
**Topic:** Basic ETL Concepts
---

### Q89. What is data profiling in the context of ETL?


**A)** Analyzing source data to understand its structure, content, quality, and patterns before building ETL pipelines
**B)** Creating user profiles in a database
**C)** Encrypting data for security
**D)** Compressing data for faster transfer
**Answer:** A — Analyzing source data to understand its structure, content, quality, and patterns before building ETL pipelines
**Explanation:** Data profiling examines source data to discover its characteristics: data types, value ranges, null percentages, unique value counts, distribution patterns, and quality issues. This analysis is done before building ETL pipelines to identify potential problems, inform transformation logic, and set expectations. Skipping profiling often leads to pipeline failures and data quality issues downstream.
**Topic:** Basic ETL Concepts
---

### Q90. What is a staging area in an ETL process?


**A)** A temporary storage area where data is held during the ETL process between extraction and loading
**B)** The final destination where users query data
**C)** The source system where data originates
**D)** A backup of the production database
**Answer:** A — A temporary storage area where data is held during the ETL process between extraction and loading
**Explanation:** A staging area is an intermediate storage location between source and target systems. Raw data is extracted into staging, where transformations are performed without impacting source systems or the target warehouse. It provides a checkpoint for data validation, allows reprocessing if errors occur, and decouples the extraction and loading phases. Staging data is typically temporary and cleared after successful loading.
**Topic:** Basic ETL Concepts
---

### Q91. What is ELT, and how does it differ from ETL?



**A)** ELT is the same as ETL, just a different spelling
**B)** In ELT, data is loaded into the target system first and then transformed there, leveraging the target system's processing power
**C)** ELT only works with spreadsheet files
**D)** ELT skips the transformation step entirely
**Answer:** B — In ELT, data is loaded into the target system first and then transformed there, leveraging the target system's processing power
**Explanation:** ELT (Extract, Load, Transform) reverses the Transform and Load order. Raw data is loaded directly into the target system (like a cloud data warehouse), then transformed using the warehouse's processing power. This approach has gained popularity with powerful cloud warehouses (Snowflake, BigQuery, Redshift) that can handle transformations efficiently. Tools like dbt embody this pattern.
**Topic:** Basic ETL Concepts

---

### Q92. Which of the following is a common ETL scheduling pattern?



**A)** Running ETL manually whenever someone remembers
**B)** Batch processing on a defined schedule (e.g., nightly at 2 AM)
**C)** Never running ETL more than once
**D)** Only running ETL when disk space runs out
**Answer:** B — Batch processing on a defined schedule (e.g., nightly at 2 AM)
**Explanation:** Batch processing on a schedule is the most common ETL pattern. Nightly runs (often during off-peak hours like 2 AM) are typical for data warehouses. The schedule depends on business needs: daily for most reporting, hourly for near-real-time dashboards, or even every few minutes for time-sensitive data. Orchestration tools like Apache Airflow, Prefect, or cron jobs manage these schedules.
**Topic:** Basic ETL Concepts

---

### Q93. What is data quality validation in ETL?


**A)** Checking that the ETL job ran without technical errors
**B)** Testing the database connection speed
**C)** Validating that users have permission to access data
**D)** Implementing checks to ensure data accuracy, completeness, consistency, and conformity after transformation
**Answer:** D — Implementing checks to ensure data accuracy, completeness, consistency, and conformity after transformation
**Explanation:** Data quality validation ensures transformed data meets defined standards before loading into the target. Common checks include: row count reconciliation (source vs. target), null checks on required fields, referential integrity validation, range checks (no negative ages), uniqueness constraints, and format validation. Catching issues early prevents bad data from reaching dashboards and business decisions.
**Topic:** Basic ETL Concepts
---

## SOFT SKILLS & COMMUNICATION (7 Questions)

---

### Q94. A business stakeholder asks: "Why did sales drop last month?" What is the best first step as a data analyst?


**A)** Immediately build a complex predictive model
**B)** Send them a raw data export from the database
**C)** Respond with "I don't know" and wait for more information
**D)** Ask clarifying questions to understand which sales metric, time period, and product lines they're referring to
**Answer:** D — Ask clarifying questions to understand which sales metric, time period, and product lines they're referring to
**Explanation:** Clarifying the question is always the first step. "Sales" could mean revenue, units sold, or number of transactions. "Last month" might mean compared to the previous month or the same month last year. Which regions or products? Understanding the exact question prevents wasted effort on the wrong analysis. Good analysts ask smart questions before diving into data.
**Topic:** Soft Skills & Communication
---

### Q95. When presenting data findings to non-technical stakeholders, which approach is most effective?


**A)** Show all your SQL queries so they understand the methodology
**B)** Use as many technical terms as possible to demonstrate expertise
**C)** Present the raw data tables and let them draw their own conclusions
**D)** Focus on insights and actionable recommendations, using clear visualizations and plain language
**Answer:** D — Focus on insights and actionable recommendations, using clear visualizations and plain language
**Explanation:** Non-technical stakeholders care about what the data means for the business, not how you extracted it. Lead with insights ("Customer churn increased 15% in Q3, driven by pricing changes"), support with clear visuals, and end with actionable recommendations. Avoid jargon, explain metrics simply, and anticipate "so what?" questions. Technical methodology can be available as an appendix if needed.
**Topic:** Soft Skills & Communication
---

### Q96. You discover that the data you've been asked to analyze has significant quality issues (30% missing values in key columns). What should you do?


**A)** Document the issues, communicate them to stakeholders, explain the potential impact, and propose solutions
**B)** Ignore the missing data and analyze what's available without mentioning it
**C)** Wait until someone else notices the problem
**D)** Fill in the missing values with random data
**Answer:** A — Document the issues, communicate them to stakeholders, explain the potential impact, and propose solutions
**Explanation:** Transparency about data quality is essential for maintaining trust and ensuring sound decisions. Document what you found, quantify the impact (e.g., "30% of customer records lack email addresses, limiting our campaign reach analysis"), and propose solutions (imputation, alternative data sources, or adjusting scope). Hiding data quality issues leads to flawed conclusions that can damage credibility and business outcomes.
**Topic:** Soft Skills & Communication
---

### Q97. A colleague asks you to manipulate a report to make the department's metrics look better than they actually are. What should you do?



**A)** Comply because they are your colleague
**B)** Refuse and explain that data integrity is paramount; misrepresenting data is unethical and can lead to poor business decisions
**C)** Do it but tell nobody
**D)** Ignore the request and hope they forget
**Answer:** B — Refuse and explain that data integrity is paramount; misrepresenting data is unethical and can lead to poor business decisions
**Explanation:** Data integrity is a core professional responsibility. Manipulating data is unethical, potentially illegal (especially for publicly traded companies), and leads to poor business decisions based on false information. Politely decline, explain why, and if pressured, escalate to your manager or data governance team. Your credibility as an analyst depends on being a trustworthy source of truth.
**Topic:** Soft Skills & Communication

---

### Q98. What is the best way to document your analysis work?



**A)** Keep all logic in your head — documentation slows you down
**B)** Write clear comments in code, maintain a methodology document, and version control your work
**C)** Only document if explicitly asked by your manager
**D)** Write documentation after the project is completely finished
**Answer:** B — Write clear comments in code, maintain a methodology document, and version control your work
**Explanation:** Good documentation serves your future self, teammates, and stakeholders. Comment your code explaining "why" (not just "what"), maintain a methodology document covering data sources, assumptions, and decisions, and use version control (Git) to track changes. Document as you go — waiting until the end means you'll forget important decisions. This is especially critical for reproducibility and knowledge transfer.
**Topic:** Soft Skills & Communication

---

### Q99. You're working on a complex analysis and realize you won't meet the deadline. What is the best approach?


**A)** Submit incomplete work at the deadline without mentioning it
**B)** Blame the data team for providing data late
**C)** Work through the night to meet the deadline regardless of quality
**D)** Communicate early with stakeholders about the delay, provide a revised timeline, and offer interim findings if possible
**Answer:** D — Communicate early with stakeholders about the delay, provide a revised timeline, and offer interim findings if possible
**Explanation:** Proactive communication builds trust even when delivering bad news. As soon as you realize a delay, inform stakeholders with: what's causing the delay, a realistic revised timeline, and what you can deliver in the meantime (partial results, preliminary findings). Most stakeholders prefer a realistic timeline over rushed, low-quality work. Early communication gives them time to adjust their own plans.
**Topic:** Soft Skills & Communication
---

### Q100. When you receive conflicting requirements from two different stakeholders, what should you do?



**A)** Choose the requirement from the more senior stakeholder
**B)** Ignore both and do what you think is best
**C)** Facilitate a conversation between both stakeholders to align on priorities and requirements
**D)** Build two separate solutions to satisfy both
**Answer:** C — Facilitate a conversation between both stakeholders to align on priorities and requirements
**Explanation:** Conflicting requirements are common and are best resolved through direct communication. Set up a meeting with both stakeholders to discuss their needs, uncover the underlying business goals (which may actually be compatible), and reach agreement on priorities. As the analyst, you provide a neutral, data-informed perspective. Building two solutions wastes effort, and unilateral decisions risk alienating a stakeholder.
**Topic:** Soft Skills & Communication

---
