# SQL-Based Data Comparison Task

This task focuses on comparing two versions of a product catalog (products_day1.csv and products_day2.csv) using SQL queries executed via Python. The goal is to identify full row additions/removals and detect column-level changes for matching rows.

## Approach

1.  **Data Ingestion:**
    * The CSV files (`products_day1.csv`, `products_day2.csv`) are loaded into pandas DataFrames.
    * An in-memory SQLite database is created using `sqlite3`.
    * The DataFrames are then ingested into this SQLite database as tables named `products_day1_tbl` and `products_day2_tbl`.

2.  **Identifying Added/Removed Rows:**
    * **Added Rows:** Found by selecting rows present in `products_day2_tbl` that do *not* exist in `products_day1_tbl` based on `product_id` and all other columns. This uses a `LEFT JOIN` and `WHERE products_day1_tbl.product_id IS NULL` clause, ensuring all columns match.
    * **Removed Rows:** Found by selecting rows present in `products_day1_tbl` that do *not* exist in `products_day2_tbl` based on `product_id` and all other columns. This also uses a `LEFT JOIN` and `WHERE products_day2_tbl.product_id IS NULL`.
    * The results for added and removed rows are then combined using `UNION ALL` and a `change_type` column is added.

3.  **Detecting Column-Level Changes:**
    * Matching rows are identified by `product_id` using an `INNER JOIN` between `products_day1_tbl` and `products_day2_tbl`.
    * For each column (`name`, `category`, `price`, `stock`), a `CASE` statement is used to check if the value in `products_day1_tbl` differs from `products_day2_tbl`.
    * If a difference is found, a row is generated including `product_id`, `column_name`, `old_value`, and `new_value`.
    * The results from checking each column are then combined using `UNION ALL`.

## Running the Solution

1.  Ensure you have Python and Jupyter Notebook installed.
2.  Place `products_day1.csv`, `products_day2.csv`, and `sql_table_comparison.ipynb` in the `sql_task/` directory.
3.  Open a terminal or command prompt in the `data-transformation-assignment` directory (or its parent).
4.  Navigate to `sql_task`: `cd sql_task`
5.  Start Jupyter Notebook: `jupyter notebook`
6.  Open `sql_table_comparison.ipynb` in your browser.
7.  Run all cells to see the results.

## Scalability and Robustness

* **No Hardcoding:** All queries dynamically compare table contents; no specific product IDs or values are hardcoded in the SQL logic.
* **Database Agnostic SQL (Mostly):** The SQL syntax used is standard SQL and should be broadly compatible with most relational databases (SQLite, PostgreSQL, MySQL, etc.) with minor adjustments if switching databases.
* **Pandas for Data Handling:** Using pandas DataFrames for CSV import and initial data handling provides flexibility and efficiency.
* **Parameterized Queries (if needed):** While not explicitly used for the main comparison logic here, `sqlite3` and `pandas.to_sql` support parameterized queries, which would be crucial for preventing SQL injection in more complex applications.