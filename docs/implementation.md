# Implementation

## Ingestion:
I created an Amazon S3 bucket called sl-my-project-raw-data. The purpose of it is to hold the raw data before transformation.
![implementation](../photos/1.png)

Then I uploaded the cart data from the github dummyjson repository with a date partition. In real life, it would be daily done and also automated using for example Lamdba function.

![implementation](../photos/2.png)
![implementation](../photos/6.png)
![implementation](../photos/7.png)

In case of the product and user data, I used the same data source but with a Lamdba funtion and uploaded to the same bucket with the appropriate folders and partitions. This Lambda would be triggered weekly using an Amazon EventBridge cron rule. Due to restricted IAM permissions in the lab environment, I could not create the trigger.
code here


![implementation](../photos/8.png)


![implementation](../photos/9.png)
![implementation](../photos/10.png)
![implementation](../photos/11.png)

## Transformation
I used Amazon Glue to create metadata about the 3 dataset. I made a new database called raw-data-db and I used a Crawler to fill the tables.

![implementation](../photos/12.png)
![implementation](../photos/13.png)
![implementation](../photos/14.png)


The Crawler was successfully run so I got the 3 tables in my database.

![implementation](../photos/15.png)
![implementation](../photos/16.png)

Also we can check the schema of each one, you can see the carts' as an example.

![implementation](../photos/17.png)

But the json format is not the best to write queries on them, so I used ETL job to change the format to parquet. It is a columnar storage file format optimized for big data processing.

In the job, I defined the new folder called cleansed and saved the newly transformed data into each of them. In case of the cart data, I transformed it to be order items because it is more generally used. 

![implementation](../photos/18.png)
![implementation](../photos/19.png)
![implementation](../photos/20.png)
![implementation](../photos/21.png)
![implementation](../photos/22.png)
![implementation](../photos/23.png)
![implementation](../photos/24.png)

I made a new database called cleansed-data-db, so the raw and transformed data are separated. All the 3 tables can be seen

![implementation](../photos/25.png)

All the 3 tables can be seen and their schemas as well.

![implementation](../photos/26.png)
![implementation](../photos/27.png)

## Analytics and reporting

### Average order value

SELECT userId, AVG(discountedTotal) AS avg_order_value FROM order_items GROUP BY userId ORDER BY avg_order_value DESC;
![implementation](../photos/28.png)
The average order value shows how much, on average, each user spends per order. Some users, like user 177 and user 33, spend very high amounts, while others spend much less. This indicates that a small number of users are placing very high-value orders, which significantly affects the overall average.

### Revenue by categories

SELECT p.category, SUM(o.discountedTotal) AS revenue FROM order_items o JOIN products p ON o.productId = p.id GROUP BY p.category ORDER BY revenue DESC;

![implementation](../photos/29.png)

The revenue by category tells us how much money each product category has generated. Categories with higher revenue are the most popular or high-priced items, while categories with lower revenue are less impactful to total income. This helps identify which product types are driving the business’s earnings.


Revenue by age group

SELECT CASE WHEN u.age < 25 THEN 'Under 25' WHEN u.age BETWEEN 25 AND 34 THEN '25-34' WHEN u.age BETWEEN 35 AND 44 THEN '35-44' ELSE '45+' END AS age_group, SUM(o.discountedTotal) AS revenue FROM order_items o JOIN users u ON o.userId = u.id GROUP BY CASE WHEN u.age < 25 THEN 'Under 25' WHEN u.age BETWEEN 25 AND 34 THEN '25-34' WHEN u.age BETWEEN 35 AND 44 THEN '35-44' ELSE '45+' END ORDER BY revenue DESC;

![implementation](../photos/30.png)

The revenue by age group shows which age segments contribute most to the total revenue. Users aged 25-34 generate the highest revenue, followed by those aged 35-44. Users aged 45 and above generate less revenue, and those under 25 contribute very little. This suggests that the business earns most of its income from adults between 25 and 34 years old, and younger customers are not a major source of revenue.

