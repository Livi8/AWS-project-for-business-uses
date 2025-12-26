# AWS-project-for-business-uses
In a production AWS environment, this Lambda would be triggered weekly using an Amazon EventBridge cron rule. Due to restricted IAM permissions in the lab environment, the trigger could not be created, but the Lambda is designed to be schedule-ready.

codes: SELECT userId, AVG(discountedTotal) AS avg_order_value
FROM order_items
GROUP BY userId
ORDER BY avg_order_value DESC;


SELECT p.category, SUM(o.discountedTotal) AS revenue
FROM order_items o
JOIN products p
ON o.productId = p.id
GROUP BY p.category
ORDER BY revenue DESC;

SELECT
  CASE
    WHEN u.age < 25 THEN 'Under 25'
    WHEN u.age BETWEEN 25 AND 34 THEN '25-34'
    WHEN u.age BETWEEN 35 AND 44 THEN '35-44'
    ELSE '45+'
  END AS age_group,
  SUM(o.discountedTotal) AS revenue
FROM order_items o
JOIN users u
ON o.userId = u.id
GROUP BY
  CASE
    WHEN u.age < 25 THEN 'Under 25'
    WHEN u.age BETWEEN 25 AND 34 THEN '25-34'
    WHEN u.age BETWEEN 35 AND 44 THEN '35-44'
    ELSE '45+'
  END
ORDER BY revenue DESC;

I would use QuickSight to make dashboards, but in my Lab environment I dont have permission to do that, so I downloaded the 3 cleansed parquet files and used them in Tableu.
