# AWS project for business uses

This project demonstrates a cloud-based data pipeline using AWS services to ingest,
transform, and analyze e-commerce sales data. I used the github dummyjson repository as data source.

My focus was to solve real life problem: using data to make better decisions.

My business use case: 
<br>-New e-commerce company growing rapidly
<br>-Business wants to use data more effectively for decisions
<br>-Expansion increases volume of product, customer, and sales data
<br>-I am helping to build a scalable data pipeline to support this need

Business requirements:
<br>-understand product performance
<br>-analyze customer behaviour
<br>-improve operational and strategic decisions

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

I would use QuickSight to make dashboards, but in my Lab environment I dont have permission to do that, so I downloaded the 3 cleansed parquet files and used them in Tableau

~ $ aws s3 cp s3://sl-my-project-raw-data/cleansed/products/date=2025-12-26/part-00000-10703912-325f-49de-bdac-3193bf8fb9c2.c000.snappy.parquet ~/data/products_2025-12-26.parquet
download: s3://sl-my-project-raw-data/cleansed/products/date=2025-12-26/part-00000-10703912-325f-49de-bdac-3193bf8fb9c2.c000.snappy.parquet to data/products_2025-12-26.parquet

~ $ aws s3 cp "s3://sl-my-project-raw-data/raw/carts/date=\"2025-12-26\"/carts (1).json" ~/data/carts_2025-12-26.json
download: s3://sl-my-project-raw-data/raw/carts/date="2025-12-26"/carts (1).json to data/carts_2025-12-26.json

~ $ aws s3 cp s3://sl-my-project-raw-data/raw/products/date=2025-12-26/data.json ~/data/products_2025-12-26.json
download: s3://sl-my-project-raw-data/raw/products/date=2025-12-26/data.json to data/products_2025-12-26.json
~ $ aws s3 cp s3://sl-my-project-raw-data/raw/users/date=2025-12-26/data.json ~/data/users_2025-12-26.json
download: s3://sl-my-project-raw-data/raw/users/date=2025-12-26/data.json to data/users_2025-12-26.json



# E-Commerce Data Pipeline on AWS

## Documentation
- [Stakeholders & Business Value](docs/stakeholders-and-value.md)
- [Pipeline Architecture](docs/pipeline-architecture.md)
- [KPIs](docs/kpis.md)
- [Implementation](docs/implementation.md)
