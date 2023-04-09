## B. Runner and Customer Experience

## 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

```sql
SELECT 
  DATE_TRUNC('week', registration_date)::DATE + 4 AS registration_week,
  COUNT(runner_id) AS runner_signup
FROM runners
GROUP BY DATE_TRUNC('week', registration_date)
ORDER BY registration_week;
```

![image](https://user-images.githubusercontent.com/104872221/230781279-0227d5b9-9ec8-4da3-8df1-7d64625c86e5.png)

## 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

```sql
WITH runner_time AS
(SELECT DISTINCT 
 order_time,
 DATE_PART('mins', AGE(order_time, pickup_time::TIMESTAMP))::INTEGER AS time_btw_order_pickup
FROM customer_orders AS c
INNER JOIN runner_orders_copy AS r
ON r.order_id = c.order_id
WHERE pickup_time IS NOT NULL
)

SELECT ROUND(AVG(-time_btw_order_pickup),3)
FROM runner_time;
```

![image](https://user-images.githubusercontent.com/104872221/230783005-5471af7e-3336-4cdf-bddc-168f432530da.png)

