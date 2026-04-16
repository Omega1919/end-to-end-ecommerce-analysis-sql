## 📂 Dataset
This dataset contains e-commerce transaction records including customer information, product details, revenue, and profit metrics. It was used to analyze business performance across multiple dimensions such as sales trends, customer behavior, and product contribution.



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
```

### Result
$1,829,230.6  

### Insight
Total revenue of **$1.83M** represents overall business performance.

---

## Revenue by Year

### Query
```sql
SELECT year, SUM(cl_price) AS total_revenue
FROM us_order_1
GROUP BY year
ORDER BY year;
```

### Result
2012 — $120,375  
2013 — $375,655  
2014 — $1,008,698  
2015 — $324,500  

### Insight
Revenue increased significantly from 2012 to 2014, peaking in 2014, before declining sharply in 2015.

---

## Year-over-Year Growth

### Query
```sql
SELECT 
    year,
    SUM(cl_price) AS revenue,
    LAG(SUM(cl_price)) OVER (ORDER BY year) AS previous_year_revenue,
    (SUM(cl_price) - LAG(SUM(cl_price)) OVER (ORDER BY year)) 
    / LAG(SUM(cl_price)) OVER (ORDER BY year) * 100 AS yoy_growth_percentage
FROM us_order_1
GROUP BY year
ORDER BY year;
```

### Result
2012 — NULL  
2013 — 212%  
2014 — 168%  
2015 — -67%  

### Insight
Revenue grew rapidly between 2012 and 2014, then declined significantly in 2015.

---

# 💸 Profitability Analysis

## Total Profit by Year

### Query
```sql
SELECT year, SUM(cl_profit) AS total_profit
FROM us_order_1
GROUP BY year
ORDER BY year;
```

### Result
2012 — $73,444  
2013 — $230,794  
2014 — $637,235  
2015 — $205,807  

### Insight
Profit followed the same trend as revenue, increasing until 2014 and declining in 2015.

---

## Profit Margin

### Query
```sql
SELECT 
    year,
    SUM(cl_profit) / SUM(cl_price) * 100 AS profit_margin
FROM us_order_1
GROUP BY year
ORDER BY year;
```

### Result
2012 — 61%  
2013 — 61%  
2014 — 63%  
2015 — 63%  

### Insight
Profit margins remained stable, indicating consistent cost efficiency.

---

# 👥 Customer Analysis

## Total Customers

### Query
```sql
SELECT COUNT(DISTINCT user_id) AS total_customers
FROM us_order_1;
```

### Result
30,040  

### Insight
The business has a large customer base but does not necessarily reflect strong retention.

---

## Customer Retention

### Query
```sql
SELECT 
    COUNT(CASE WHEN is_primary_item = 1 THEN 1 END) AS primary_orders,
    COUNT(CASE WHEN is_primary_item = 0 THEN 1 END) AS secondary_orders
FROM order_items;
```

### Result
Primary Orders — 32,313  
Secondary Orders — 7,712  

### Insight
Most customers make only one purchase, indicating low retention.

---

## Top 10 Customers Contribution

### Query
```sql
SELECT SUM(total_revenue) 
FROM (
    SELECT user_id, SUM(cl_price) AS total_revenue
    FROM us_order_1
    GROUP BY user_id
    ORDER BY total_revenue DESC
    LIMIT 10
) bb;
```

### Result
$2,203.56  

### Insight
Revenue is widely distributed, with little contribution from top customers.

---

# 📦 Order Analysis

## Total Orders

### Query
```sql
SELECT COUNT(order_item_id) AS total_orders
FROM s_order;
```

### Result
37,739  

### Insight
Most customers place only one order.

---

## Average Order Value (AOV)

### Query
```sql
SELECT AVG(price_usd) AS AOV
FROM order_items;
```

### Result
48.4  

### Insight
Average customer spending per order is $48.4.

---

# 🔁 Refund Analysis

## Total Refund Orders

### Query
```sql
SELECT COUNT(order_item_refund_id) AS total_refund_orders
FROM order_item_refunds;
```

### Result
1,731  

### Insight
Refund rate is moderate and impacts revenue.

---

## Total Refund Loss

### Query
```sql
SELECT SUM(refund_amount_usd) AS total_loss
FROM order_item_refunds;
```

### Result
$85,338.69  

### Insight
Refunds caused a significant revenue loss.

---

# 🛍️ Product Analysis

## Product Performance

### Query
```sql
SELECT 
    product_name, 
    SUM(cl_price) AS total_revenue,
    COUNT(order_order_id) AS total_orders
FROM us_order_1
GROUP BY product_name
ORDER BY total_revenue DESC;
```

### Result
The Original Mr. Fuzzy — $1,141,071 — 22,826  
The Forever Mini Bear — $335,284 — 5,589  
The Birthday Sugar Panda — $210,542 — 4,578  
The Hudson River Mini Bear — $142,332 — 4,746  

### Insight
The Original Mr. Fuzzy dominates both revenue and order volume.

---

## Product Contribution

### Query
```sql
SELECT 
    product_name,
    SUM(cl_price) AS total_revenue,
    SUM(cl_price) * 100 / (SELECT SUM(cl_price) FROM us_order_1) AS product_contribution
FROM us_order_1
GROUP BY product_name
ORDER BY product_contribution DESC;
```

### Result
The Original Mr. Fuzzy — 62.2%  
The Forever Mini Bear — 18.3%  
The Birthday Sugar Panda — 11.5%  
The Hudson River Mini Bear — 8%  

### Insight
Revenue is highly concentrated in one product, creating dependency risk.

---

# 🔥 Key Findings

- Strong growth until 2014, followed by decline in 2015  
- Stable profit margins despite revenue changes  
- Low customer retention  
- High dependency on a single product  
- Refunds reduce overall revenue  

---

# 📌 Conclusion

The business shows strong growth potential but faces challenges in customer retention and product dependency. Addressing these issues can improve long-term stability.

# 📊 Dashboard

[View Interactive Dashboard](https://datastudio.google.com/reporting/23fe2dfd-1216-4a25-bc48-96980879448c)

[View Interactive Dashboard Image](end-end_dashboard.png)
