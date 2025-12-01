# Copy-of-Dmart-analysis-using-pyspark
DMart Sales & Customer Analytics — PySpark Project
1. Introduction

This project implements a PySpark-based analytics pipeline for a retail chain (DMart), integrating sales, product, and customer data.

The workflow extracts, cleans, transforms, and analyzes the datasets to generate key business insights such as sales by category, top customers, regional product distribution, profit margins, and shipping performance.

The project is fully executable in Google Colab and outputs the results as CSV files in the /content/output/ directory.

2. Objectives

Data Cleaning & Normalization: Ensure consistency in column names, data types, and missing values.

Data Integration: Join sales, product, and customer datasets on appropriate keys.

Feature Engineering: Calculate sales_amount, unit_price, and profit margins.

Business Insights: Generate analytics for top customers, sales by category/sub-category, regional performance, and shipping mode analysis.

Output Generation: Save all query results as CSV for further analysis or reporting.

3. Dataset

Product.csv: Contains product_id, category, sub_category, product_name.

Sales.csv: Contains order_id, customer_id, product_id, sales, quantity, discount, profit, order_date, ship_mode.

Customer.csv: Contains customer_id, customer_name, segment, age, region, city, state, postal_code, country.

Note: Users can upload these files via Colab or store them in /content/data/.

4. Tools & Technologies
Tool	Purpose
Python 3	Core scripting
PySpark 3.5.1	Distributed data processing
Google Colab	Cloud-based execution environment
Pandas	Optional CSV post-processing
pathlib, shutil, os	File handling
Google Colab files	File upload/download
Spark SQL	Querying and aggregation
Window functions	Generating surrogate keys
5. Project Folder Structure
DMart_PySpark/
│
├── Product.csv           # Product details
├── Sales.csv             # Sales transaction data
├── Customer.csv          # Customer master data
├── DMart_PySpark_Colab.ipynb  # Main Colab notebook (PySpark ETL & analytics)
├── output/               # CSV output for each query
└── README.md             # Project documentation

6. Steps to Run the Project
Step 1: Setup Colab Environment
# Install Java & PySpark
!apt-get install openjdk-11-jdk -qq
!pip install pyspark==3.5.1 findspark

Step 2: Initialize Spark
import findspark
findspark.init()

from pyspark.sql import SparkSession
spark = SparkSession.builder.master("local[*]").appName("DMart_PySpark_Final").getOrCreate()

Step 3: Upload / Load Dataset

Place files in /content/data/ or upload via Colab's file uploader.

from google.colab import files
# files.upload() prompts user to upload Product.csv, Sales.csv, Customer.csv

Step 4: Data Cleaning & Transformation

Normalize column names

Rename key columns (product_id, customer_id)

Cast numeric fields (sales, quantity, profit, discount)

Generate missing fields (unit_price, surrogate keys)

Convert dates to proper format

Step 5: Join DataFrames

Join Sales → Product → Customer

Compute sales_amount = quantity * unit_price

Step 6: Run Analytics Queries

Example queries included:

sales_by_category

top_customer_orders

avg_discount

unique_products_region

profit_by_state

top_subcategory_sales

avg_age_segment

orders_by_shipmode

qty_by_city

profit_margin_segment

Step 7: Save Output

All query results are saved in /content/output/ as CSV files.

df_res.coalesce(1).write.mode("overwrite").option("header",True).csv("output/query_name.csv")

7. Sample Output
Query	Sample Result
sales_by_category	Total sales per category (Technology, Furniture, Office Supplies)
top_customer_orders	Top 10 customers by order count
avg_discount	Average discount across all sales
profit_by_state	Total profit per state
top_subcategory_sales	Top 10 selling subcategories
avg_age_segment	Average age of customers per segment
orders_by_shipmode	Orders by shipping mode
qty_by_city	Quantity sold per city
profit_margin_segment	Profit and margin per customer segment
8. Key Features

End-to-end PySpark ETL: Load → Clean → Transform → Join → Analyze

Safe key handling: Surrogate keys generated when missing

Unit Price Computation: Handles missing unit_price field

Dynamic Queries: Easy to add new SQL-based analytics

Output CSVs: Ready for reporting or dashboard ingestion

9. Future Improvements

Integrate real-time streaming for live sales analytics

Implement interactive dashboards with Plotly or Tableau

Add predictive analytics models for sales forecasting

Extend Airflow orchestration for automated periodic runs

10. Notes

Fully Colab-compatible with PySpark 3.5.1

Works with both uploaded files or pre-stored files in /content/data/

Output saved in /content/output/ for download
