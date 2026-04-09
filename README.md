# data-cleaning-validation-project
Data entry, cleaning, and validation project using SQL and Excel to transform raw sales data into a structured and analysis-ready dataset.

## Objective
This project simulates a real freelance task involving data entry, cleaning, and validation. The objective is to transform raw, unstructured sales data into a clean and reliable dataset that is ready for analysis.

## Raw Data Issues
The dataset contained several data quality problems:
- Duplicate records
- Missing values (NULL)
- Inconsistent date formats
- Incomplete transaction data

## Data Cleaning Process (SQL)
Key steps performed:
- Removed duplicate records using DISTINCT
- Standardized date format using TO_DATE
- Handled missing values using COALESCE
- Filtered out invalid records (missing date)

### SQL query
WITH cleaned AS (
    SELECT DISTINCT
        order_id,
        customer_name,
        TO_DATE(REPLACE(order_date, '/', '-'), 'YYYY-MM-DD') AS order_date,
        product_name,
        category,
        COALESCE(quantity, 1) AS quantity,
        COALESCE(price, 0) AS price
    FROM sales_raw
    WHERE order_date IS NOT NULL
)
SELECT *,
       quantity * price AS total_price
FROM cleaned;

## Data Validation
After cleaning, validation checks were performed to ensure:
- No duplicate records
- No critical missing values
- Consistent data formats

## Analysis
### Total Revenue
''' SQL
SELECT 
    SUM(total_price) AS total_revenue
FROM cleaned_data;

### Top Product
''' SQL
SELECT 
    product_name,
    SUM(quantity) AS total_units_sold
FROM cleaned_data
GROUP BY product_name
ORDER BY total_units_sold DESC;

### Top Customers
''' SQL
SELECT 
    customer_name,
    SUM(total_price) AS total_spending
FROM cleaned_data
GROUP BY customer_name
ORDER BY total_spending DESC;

## Key Insight
Key Insights
- Data cleaning is essential before performing any analysis
- A small number of products contribute the highest sales volume
- Certain customers generate higher revenue than others
- Removing invalid data improves analysis accuracy

## Conclusion
This project the importance of data cleaning and validation in ensuring accurate and reliable analysis.
By transforming raw data into a structured format, the dataset becomes ready for business use and decision-making.
