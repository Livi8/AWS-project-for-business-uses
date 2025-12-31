# Implementation

## Ingestion:
I created an Amazon S3 bucket called sl-my-project-raw-data. The purpose of it is to hold the raw data before transformation.
![implementation](../photos/1.png)

Then I uploaded the cart data from the github dummyjson repository with a date partition. In real life, it would be daily done and also automated using for example Lamdba function.

![implementation](../photos/2.png)
![implementation](../photos/6.png)
![implementation](../photos/7.png)

In case of the product and user data, I used the same data source but with a lamdba funtion and uploaded to the same bucket with the appropriate folders and partitions. code here


![implementation](../photos/8.png)


![implementation](../photos/9.png)
![implementation](../photos/10.png)
![implementation](../photos/11.png)

## Transformation
I used Amazon Glue to create metadata about the 3 dataset. I made a new database called raw-data-db and I used a Crawler to fill the tables.

![implementation](../photos/12.png)
![implementation](../photos/13.png)
![implementation](../photos/14.png)
![implementation](../photos/15.png)
![implementation](../photos/16.png)
![implementation](../photos/17.png)
![implementation](../photos/18.png)
![implementation](../photos/19.png)
![implementation](../photos/20.png)
![implementation](../photos/21.png)
![implementation](../photos/22.png)
![implementation](../photos/23.png)
![implementation](../photos/24.png)
![implementation](../photos/25.png)
![implementation](../photos/26.png)
![implementation](../photos/27.png)
![implementation](../photos/28.png)
![implementation](../photos/29.png)
![implementation](../photos/30.png)
