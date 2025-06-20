# Python-Based Data Cleaning and Transformation Task

This task involves cleaning and transforming a messy sales dataset (`raw_sales.csv`) using Python, primarily with the pandas library. The goal is to standardize data types, handle missing/invalid values, and derive a new column.

## Approach

1.  **Data Ingestion:**
    * The `raw_sales.csv` file is loaded into a pandas DataFrame.

2.  **Data Cleaning (`clean_sales_data` function):**
    * **Type Conversion:**
        * `order_id`, `product_id`: Converted to integer. Missing or non-numeric values are handled by attempting conversion, and errors are coerced to `NaN` before filling with `0`. Invalid `product_id` (like negative values) could be flagged or treated as missing/invalid, here we're converting invalid to NaN and then to 0.
        * `quantity`: Converted to integer. Non-numeric values (including empty strings) are treated as `NaN` and then filled with `0`.
        * `price_per_unit`: Converted to float. Non-numeric values (including empty strings) are treated as `NaN` and then filled with `0.0`.
        * `order_date`: Converted to datetime objects using `pd.to_datetime`. Errors (invalid date formats) are coerced to `NaT` (Not a Time) and then handled (e.g., filled with a default date if required, though the prompt implies blank/0 which is tricky for dates. For this solution, we will leave `NaT` for missing dates).
    * **Handling Missing/Invalid Values:**
        * For numeric columns (`order_id`, `product_id`, `quantity`, `price_per_unit`), `NaN` values resulting from failed conversions are filled with `0`.
        * For `order_date`, `NaT` values are left as `NaT` as there's no clear "0" equivalent for dates.

3.  **Total Price Calculation (UDF: `calculate_total_price`):**
    * A user-defined function (`calculate_total_price`) is created to compute `quantity * price_per_unit`. This function includes error handling to ensure it works even if the inputs are not numeric.
    * This UDF is then applied to create the new `total_price` column.

4.  **Output:**
    * The final cleaned DataFrame is saved to `cleaned_sales.csv`.

## Running the Solution

1.  Ensure you have Python and Jupyter Notebook installed, along with the `pandas` library (`pip install pandas`).
2.  Place `raw_sales.csv` and `python_data_cleaning.ipynb` in the `python_da_task/` directory.
3.  Open a terminal or command prompt in the `data-transformation-assignment` directory (or its parent).
4.  Navigate to `python_da_task`: `cd python_da_task`
5.  Start Jupyter Notebook: `jupyter notebook`
6.  Open `python_data_cleaning.ipynb` in your browser.
7.  Run all cells to execute the data cleaning and transformation, and generate `cleaned_sales.csv`.

## Scalability and Robustness

* **Functions for Logic:** All cleaning and transformation steps are encapsulated within functions, promoting reusability and modularity.
* **No Hardcoding:** The code dynamically processes columns and handles various data inconsistencies without hardcoding specific problematic values or rows.
* **Pandas for Robustness:** Pandas' `read_csv`, `to_numeric`, `to_datetime`, and `fillna` functions are robust in handling different data types and missing values. The `errors='coerce'` parameter is crucial for this.
* **Error Handling in UDF:** The `calculate_total_price` UDF includes a `try-except` block to gracefully handle non-numeric inputs.