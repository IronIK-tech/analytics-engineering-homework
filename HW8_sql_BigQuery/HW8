-- Завдання 1
SELECT
  country,
  COUNT(id) AS users_count
FROM `bigquery-public-data.thelook_ecommerce.users`
GROUP BY country
ORDER BY users_count DESC
LIMIT 10;

-- Завдання 2
SELECT
  category,
  COUNT(product_id) AS  items_sold,
  ROUND(SUM(oi.sale_price), 2) AS revenue
FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
 ON oi.product_id = p.id
GROUP BY p.category
ORDER BY SUM(oi.sale_price) desc; -- шоб зручніше було читати

-- Завдання 3
SELECT
  status,
  COUNT(status) AS orders_count
FROM `bigquery-public-data.thelook_ecommerce.orders`
WHERE status in ('Complete', 'Shipped')
GROUP BY status;

-- Завдання 4
SELECT
  FORMAT_DATE('%Y-%m', DATE(created_at))AS month,
  COUNT(product_id) AS items_sold,
  ROUND(AVG(sale_price), 2) AS avg_price
FROM `bigquery-public-data.thelook_ecommerce.order_items`
WHERE EXTRACT(YEAR FROM created_at) in (2024, 2025)
GROUP BY month
ORDER BY month asc;

-- Завдання 5
SELECT
  category,
  ROUND(SUM(oi.sale_price), 2) AS revenue
FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
 ON oi.product_id = p.id
GROUP BY p.category
having sum(oi.sale_price) > 100000
ORDER BY SUM(oi.sale_price) desc;

-- Завдання 6
SELECT
  p.name AS product_name,
  p.brand,
  COUNT(oi.order_id) AS times_sold,
  ROUND(SUM(oi.sale_price), 2) AS revenue
FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
  ON oi.product_id = p.id
GROUP BY p.name, p.brand
ORDER BY revenue desc
LIMIT 10;

-- Завдання 7
SELECT
  p.category,
  COUNT(oi.id) AS total_items,
  COUNTIF(oi.returned_at IS NOT NULL) AS  returned_items,
  ROUND(COUNTIF(oi.returned_at  IS NOT NULL) / COUNT(oi.id) * 100, 1) AS return_rate_pct
FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
  ON oi.product_id = p.id
GROUP BY p.category
ORDER BY return_rate_pct desc;

-- Завдання 8
WITH category_buyers AS (
  SELECT DISTINCT
    p.category  AS category,
    oi.user_id AS user_id,
    u.age AS age
  FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
  JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
    ON oi.product_id = p.id
  JOIN `bigquery-public-data.thelook_ecommerce.users` AS u
    ON oi.user_id = u.id
)
SELECT
  category,
  COUNT(user_id) AS buyers,
  ROUND(AVG(age), 2) AS avg_age
FROM category_buyers
GROUP BY category
ORDER BY avg_age desc;

-- Завдання 9
WITH monthly_revenue AS (
  SELECT
    FORMAT_DATE('%Y-%m', DATE(created_at)) AS month,
    ROUND(SUM(sale_price), 2) AS revenue
  FROM `bigquery-public-data.thelook_ecommerce.order_items`
  WHERE EXTRACT(YEAR FROM created_at) = 2025
  GROUP BY month
)
SELECT
month,
revenue,
  RANK() OVER (ORDER BY revenue DESC) AS revenue_rank
FROM monthly_revenue
ORDER BY revenue_rank asc;

-- Завдання 10
WITH monthly_revenue AS (
  SELECT
    FORMAT_DATE('%Y-%m', DATE(created_at)) AS month,
    ROUND(SUM(sale_price), 2) AS revenue
FROM `bigquery-public-data.thelook_ecommerce.order_items`
  WHERE EXTRACT(YEAR FROM created_at) = 2025
  GROUP BY month
),
with_prev AS (
  SELECT
   month,
   revenue,
    LAG(revenue) OVER (ORDER BY month) AS prev_month_revenue
  FROM monthly_revenue
)
SELECT
month,
revenue,
prev_month_revenue,
ROUND((revenue - prev_month_revenue) / prev_month_revenue* 100, 1) AS growth_pct
FROM with_prev
ORDER BY month asc;

-- Завдання 11
WITH product_revenue AS (
  SELECT
    p.category AS category,
    p.name AS product_name,
    ROUND(SUM(oi.sale_price), 2) AS revenue
  FROM `bigquery-public-data.thelook_ecommerce.order_items` AS oi
  JOIN `bigquery-public-data.thelook_ecommerce.products` AS p
    ON oi.product_id = p.id
  GROUP BY category, product_name
)
SELECT
category,
product_name,
revenue,
  ROW_NUMBER() OVER (PARTITION BY category ORDER BY revenue DESC) AS rank_in_category
FROM product_revenue
QUALIFY rank_in_category <= 3
ORDER BY category, rank_in_category;
