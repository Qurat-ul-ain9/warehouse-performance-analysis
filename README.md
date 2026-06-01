# warehouse-performance-analysis
This analysis evaluates warehouse-level order fulfillment using SQL in Google BigQuery. It joins warehouse and order datasets to compute order counts per warehouse and their percentage contribution to total orders. Warehouses are segmented into performance tiers to highlight distribution patterns and operational efficiency.
# Warehouse Order Fulfillment Analysis (SQL | BigQuery)

## Project Overview

This project analyzes warehouse performance using SQL in Google BigQuery. The objective is to understand how different warehouses contribute to total order fulfillment and to categorize them based on their performance levels.

By combining warehouse and order data, this analysis provides insights into operational efficiency and distribution of order volumes across warehouses.

## Tools and Technologies

* Google BigQuery
* SQL (Standard SQL)
* Data Analysis and Aggregation
* Dataset: warehouse_orders (BigQuery sample dataset)

## Objectives

* Calculate the number of orders fulfilled by each warehouse
* Compare individual warehouse performance against total orders
* Categorize warehouses based on fulfillment contribution
* Identify high-performing and low-performing warehouses

## Approach and Methodology

* Joined warehouse and orders datasets using warehouse_id
* Calculated order counts per warehouse using COUNT()
* Used a subquery to calculate total orders across all warehouses
* Computed each warehouse’s percentage contribution to total orders
* Applied CASE logic to classify warehouses into performance groups
* Filtered results to include only warehouses with at least one order

## SQL Concepts Used

* JOIN (LEFT JOIN between warehouse and orders tables)
* Aggregation using COUNT()
* Subqueries for total calculations
* String formatting using CONCAT()
* Conditional logic using CASE WHEN
* GROUP BY for summarization
* HAVING for filtering aggregated results

## Key Insights

* Warehouses do not contribute equally to total order fulfillment
* A small number of warehouses handle a larger share of total orders
* Performance classification helps identify operational differences across warehouses
* The analysis can support decision-making in logistics and resource allocation

## SQL Query

```sql id="8kq2ld"
SELECT
  Warehouse.warehouse_id,
  CONCAT(Warehouse.state, ': ', Warehouse.warehouse_alias) AS warehouse_name,
  COUNT(Orders.order_id) AS number_of_orders,
  (SELECT COUNT(*) 
   FROM bigquerydemo-495713.warehouse_orders.orders AS Orders) AS total_orders,
  CASE
    WHEN COUNT(Orders.order_id)/(SELECT COUNT(*) FROM bigquerydemo-495713.warehouse_orders.orders AS Orders) <= 0.20
    THEN 'Fulfilled 0-20% of Orders'
    WHEN COUNT(Orders.order_id)/(SELECT COUNT(*) FROM bigquerydemo-495713.warehouse_orders.orders AS Orders) <= 0.60
    THEN 'Fulfilled 21-60% of Orders'
    ELSE 'Fulfilled more than 60% of Orders'
  END AS fulfillment_summary
FROM bigquerydemo-495713.warehouse_orders.warehouse AS Warehouse
LEFT JOIN bigquerydemo-495713.warehouse_orders.orders AS Orders
ON Orders.warehouse_id = Warehouse.warehouse_id
GROUP BY
  Warehouse.warehouse_id,
  warehouse_name
HAVING
  COUNT(Orders.order_id) > 0;
```

## Key Learnings

* Working with relational datasets in Google BigQuery
* Writing analytical SQL queries for real-world problems
* Using conditional logic for data classification
* Translating raw data into business insights
* Understanding warehouse performance distribution

## Future Improvements

* Add time-based trend analysis (monthly or quarterly performance)
* Create visual dashboards using Tableau or Power BI
* Refactor query using CTEs for improved readability
* Expand analysis to include delivery time and efficiency metrics

## Feedback

This project is part of a data analytics learning journey. Feedback and suggestions are welcome.
