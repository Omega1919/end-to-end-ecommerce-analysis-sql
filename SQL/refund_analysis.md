# 🔁 Refund Analysis

## Total Refund Orders
```sql
SELECT COUNT(order_item_refund_id) AS total_refund_orders
FROM order_item_refunds;
```

## Total Refund Loss
```sql
SELECT SUM(refund_amount_usd) AS total_loss
FROM order_item_refunds;
```
