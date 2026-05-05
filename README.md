# Product-Data-Cleaning-Median-Imputation
This project focuses on data cleaning, transformation, and imputation for a product dataset using SQL. The goal is to standardize inconsistent fields, handle missing values, and compute robust median-based imputations for numeric columns such as weight and price.

🎯 **Objectives**
Clean and standardize product data fields
Extract numeric values from text-based columns
Compute medians using window functions
Impute missing values using median substitution
Normalize categorical fields for consistency
🗂 Dataset Assumptions

The query operates on a table named:

products
Expected Columns:
Column Name	Description
product_id	Unique product identifier
product_type	Type/category of product
brand	Brand name
weight	Product weight (text or numeric)
price	Product price
average_units_sold	Average units sold
year_added	Year product was added
stock_location	Inventory location

⚙️ **Key Features**
1. 🧹 Weight Cleaning & Standardization
Handles values like:
"500 grams"
"500"
Uses regex to extract numeric values
Converts text → numeric format
WHEN weight ~ E'^\\d+\\.?\\d*\\s*grams$'
2. 📊 Median Calculation (Robust Statistics)

Median is calculated using window functions:

ROW_NUMBER() OVER (ORDER BY weight::NUMERIC)
COUNT(*) OVER()
Supports both odd and even row counts
Uses:
WHERE rn IN ((cnt + 1) / 2, (cnt + 2) / 2)
3. 🔄 Missing Value Imputation
Missing or invalid values replaced with:
Median weight
Median price
COALESCE(value, median_value)
4. 🧼 Categorical Data Cleaning

Standardizes text fields:

Field	Cleaning Logic
product_type	Empty → 'Unknown'
brand	'-' → 'Unknown'
stock_location	Empty → 'Unknown', converted to UPPER
5. 🔢 Default Value Handling
Column	Default Value
average_units_sold	0
year_added	2022

**🧠 Why Median Instead of Mean?**

Median is used because it is:

Resistant to outliers
More stable for skewed distributions
Better suited for real-world product data

🏗 **Query Structure**

The query is organized using Common Table Expressions (CTEs):

ranked_weights → Orders valid weights
median_weight → Computes median weight
ranked_prices → Orders prices
median_price → Computes median price
Final SELECT → Cleans and imputes data

**🚀 How to Use**
Ensure your database contains the products table
Run the SQL query in:
PostgreSQL
Databricks SQL (with minor syntax adjustments)
Any SQL engine supporting window functions

📈 **Example Output**
product_id	product_type	brand	weight	price	units_sold	year	location
101	Electronics	Sony	500.00	99.99	25	2023	A1
102	Unknown	Unknown	450.00	79.99	0	2022	UNKNOWN
🛠 Tech Stack
SQL (PostgreSQL syntax)
Window Functions
Regular Expressions (Regex)
Data Cleaning & Transformation Techniques

💡 **Future Improvements**
Add unit conversion (grams → kg)
Handle additional formats (e.g., "0.5 kg")
Build a dbt pipeline for production use
Integrate into a full ETL workflow (Airflow / Spark)

**👤 Author**

Antoine Ward
📍 Washington, DC Metro Area
🔗 GitHub: https://github.com/antoinewrd1

🔗 LinkedIn: https://linkedin.com/in/antoine-ward-mph-2401581a1
