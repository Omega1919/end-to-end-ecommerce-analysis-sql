# 📊 E-Commerce Data Analysis Report

## 🧾 Overview

This project analyzes e-commerce transaction data to evaluate business performance across revenue, customer behavior, product performance, and profitability. The goal is to identify key trends, growth patterns, and potential risks impacting the business.

---

# 💰 Revenue Analysis

## Total Revenue

### Query
```sql
SELECT SUM(cl_price) AS total_revenue
FROM us_order_1;
Result
$1,829,230.6

Insight
Total revenue of $1.83M represents overall business performance. However, without a benchmark or prior comparison, this alone does not fully indicate growth or decline.
Revenue by Year
Query
SQL
SELECT year, SUM(cl_price) AS total_revenue
FROM us_order_1
GROUP BY year
ORDER BY year;
Insight
Revenue increased significantly from 2012 to 2014, peaking at over $1M in 2014. However, there was a sharp decline in 2015, indicating a potential drop in business performance that requires further investigation.
Year-over-Year Growth
Query
SQL
SELECT 
    year,
    SUM(cl_price) AS revenue,
    LAG(SUM(cl_price)) OVER (ORDER BY year) AS previous_year_revenue,
    (SUM(cl_price) - LAG(SUM(cl_price)) OVER (ORDER BY year)) 
    / LAG(SUM(cl_price)) OVER (ORDER BY year) * 100 AS yoy_growth_percentage
FROM us_order_1
GROUP BY year
ORDER BY year;
Insight
Year-over-year growth analysis reveals rapid expansion between 2012 and 2014, with revenue growing by 212% in 2013 and 168% in 2014. However, this growth was not sustained, as revenue declined sharply by 67% in 2015, suggesting a significant drop in demand or business activity after the 2014 peak.
💸 Profitability Analysis
Total Profit by Year
Query
SQL
SELECT year, SUM(cl_profit) AS total_profit
FROM us_order_1
GROUP BY year
ORDER BY year;
Insight
Profit followed a similar trend to revenue, increasing from 2012 to 2014 before declining in 2015. This indicates that changes in revenue directly impacted overall profitability.
Profit Margin
Query
SQL
SELECT 
    year,
    SUM(cl_profit) / SUM(cl_price) * 100 AS profit_margin
FROM us_order_1
GROUP BY year
ORDER BY year;
Insight
Profit margins remained stable between 61% and 63% across all years, indicating consistent cost efficiency despite fluctuations in revenue and profit.
👥 Customer Analysis
Total Customers
Query
SQL
SELECT COUNT(DISTINCT user_id) AS total_customers
FROM us_order_1;
Result
30,040
Insight
The business has a large customer base of over 30,000 customers, but this does not necessarily indicate strong customer retention.
Customer Retention
Query
SQL
SELECT 
    COUNT(CASE WHEN is_primary_item = 1 THEN 1 END) AS primary_orders,
    COUNT(CASE WHEN is_primary_item = 0 THEN 1 END) AS secondary_orders
FROM order_items;
Insight
Most customers appear to make only one purchase, even when buying multiple items within a single order. This indicates low customer retention and limited repeat purchasing behavior.
Top 10 Customers Contribution
Query
SQL
SELECT SUM(total_revenue) 
FROM (
    SELECT user_id, SUM(cl_price) AS total_revenue
    FROM us_order_1
    GROUP BY user_id
    ORDER BY total_revenue DESC
    LIMIT 10
) bb;
Result
$2,203.56
Insight
Revenue contribution from the top 10 customers is extremely low compared to total revenue, indicating that revenue is widely distributed across customers rather than concentrated among a few high-value buyers.
📦 Order Analysis
Total Orders
Query
SQL
SELECT COUNT(order_item_id) AS total_orders
FROM s_order;
Result
37,739
Insight
The number of orders is relatively close to the number of customers, further confirming that most customers place only one order.
Average Order Value (AOV)
Query
SQL
SELECT AVG(price_usd) AS AOV
FROM order_items;
Result
48.4
Insight
The average order value of $48.4 reflects typical customer spending per transaction. However, inclusion of refunded items may slightly affect this value.
🔁 Refund Analysis
Total Refund Orders
Query
SQL
SELECT COUNT(order_item_refund_id) AS total_refund_orders
FROM order_item_refunds;
Result
1,731
Insight
Refunds account for approximately 4.32% of total orders, representing a moderate return rate that may impact profitability.
Total Refund Loss
Query
SQL
SELECT SUM(refund_amount_usd) AS total_loss
FROM order_item_refunds;
Result
$85,338.69
Insight
Refunds resulted in a total loss of over $85K, highlighting a noticeable reduction in revenue due to returned items.
🛍️ Product Analysis
Product Performance
Query
SQL
SELECT 
    product_name, 
    SUM(cl_price) AS total_revenue,
    COUNT(order_order_id) AS total_orders
FROM us_order_1
GROUP BY product_name
ORDER BY total_revenue DESC;
Insight
The Original Mr. Fuzzy is the top-performing product, generating over $1.14M in revenue and the highest number of orders. In contrast, the Hudson River Mini Bear generates the lowest revenue despite having a relatively high order count, indicating a lower price point.
Product Contribution
Query
SQL
SELECT 
    product_name,
    SUM(cl_price) AS total_revenue,
    SUM(cl_price) * 100 / (SELECT SUM(cl_price) FROM us_order_1) AS product_contribution
FROM us_order_1
GROUP BY product_name
ORDER BY product_contribution DESC;
Insight
Revenue is highly concentrated in a single product, with The Original Mr. Fuzzy contributing approximately 62% of total revenue. This indicates a strong dependency on one product, which poses a potential business risk if demand declines.
🔥 Key Findings
Revenue and profit grew rapidly between 2012 and 2014 but declined sharply in 2015
Profit margins remained stable, indicating consistent operational efficiency
Customer retention is low, with most customers making only one purchase
Revenue is highly dependent on a single product
Refunds contribute to noticeable revenue loss
Revenue is widely distributed across customers
📌 Conclusion
The business experienced strong growth followed by a significant decline, suggesting potential issues in sustaining performance. While operational efficiency remains stable, key challenges include low customer retention and high dependency on a single product. Addressing these areas could improve long-term business stability and growth.
