## Module 2 Homework
The homework link is [here](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/cohorts/2024/03-data-warehouse/homework.md)

# Table of Contents
- [Overview](#overview)
- [Question 1](#q1)
- [Question 2](#q2)
- [Question 3](#q3)
- [Question 4](#q4)
- [Question 5](#q5)
- [Question 6](#q6)
- [Question 7](#q7)

<a id="overview"></a>
## Overview

<b>SETUP:</b></br>
Create an external table using the Green Taxi Trip Records Data for 2022. </br>
Create a table in BQ using the Green Taxi Trip Records for 2022 (do not partition or cluster this table). </br>
</p>

``` SQL
CREATE EXTERNAL TABLE IF NOT EXISTS week3_zoomcamp.external_green

OPTIONS(
format = 'PARQUET',
uris = ['gs://week3-bucketerino-parquet/green/*.parquet']
);

CREATE TABLE IF NOT EXISTS week3_zoomcamp.native_green AS
SELECT * FROM week3_zoomcamp.external_green;
```
<a id="q1"></a>

## Question 1:
Question 1: What is count of records for the 2022 Green Taxi Data??
- 65,623,481
- 840,402
- 1,936,423
- 253,647
### Answer: `840,402`
```SQL
SELECT COUNT(1) FROM `stoked-champion-410819.week3_zoomcamp.native_green`
-- 840,402
```
<a id="q2"></a>

## Question 2:
Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.</br> 
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

- 0 MB for the External Table and 6.41MB for the Materialized Table
- 18.82 MB for the External Table and 47.60 MB for the Materialized Table
- 0 MB for the External Table and 0MB for the Materialized Table
- 2.14 MB for the External Table and 0MB for the Materialized Table
### Answer: `0 MB for the External Table and 6.41MB for the Materialized Table`
```SQL
-- native:
SELECT COUNT(DISTINCT(PULocationID)) FROM `stoked-champion-410819.week3_zoomcamp.native_green`
-- 6.41MB

-- external:
SELECT COUNT(DISTINCT(PULocationID)) FROM `stoked-champion-410819.week3_zoomcamp.external_green`
-- 0B
```
<a id="q3"></a>

## Question 3:
How many records have a fare_amount of 0?
- 12,488
- 128,219
- 112
- 1,622
### Answer: `1,622`
```SQL
SELECT COUNT(1) FROM `stoked-champion-410819.week3_zoomcamp.native_green` WHERE fare_amount = 0.0
-- 1,622
```
<a id="q4"></a>

## Question 4:
What is the best strategy to make an optimized table in Big Query if your query will always order the results by PUlocationID and filter based on lpep_pickup_datetime? (Create a new table with this strategy)
- Cluster on lpep_pickup_datetime Partition by PUlocationID
- Partition by lpep_pickup_datetime Cluster on PUlocationID
- Partition by lpep_pickup_datetime and Partition by PUlocationID
- Cluster on by lpep_pickup_datetime and Cluster on PUlocationID
### Answer: `Partition by lpep_pickup_datetime Cluster on PUlocationID`
```SQL
CREATE TABLE week3_zoomcamp.green_partitioned_clustered
PARTITION BY DATE(lpep_pickup_datetime)
CLUSTER BY PUlocationID AS
SELECT * FROM week3_zoomcamp.native_green;
```
<a id="q5"></a>

## Question 5:
Write a query to retrieve the distinct PULocationID between lpep_pickup_datetime
06/01/2022 and 06/30/2022 (inclusive)</br>

Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 4 and note the estimated bytes processed. What are these values? </br>

Choose the answer which most closely matches.</br> 

- 22.82 MB for non-partitioned table and 647.87 MB for the partitioned table
- 12.82 MB for non-partitioned table and 1.12 MB for the partitioned table
- 5.63 MB for non-partitioned table and 0 MB for the partitioned table
- 10.31 MB for non-partitioned table and 10.31 MB for the partitioned table
### Answer: `12.82 MB for non-partitioned table and 1.12 MB for the partitioned table`

```SQL
-- non-partitioned:
SELECT DISTINCT(PULocationID) FROM `stoked-champion-410819.week3_zoomcamp.native_green`
WHERE DATE(lpep_pickup_datetime)
        BETWEEN
          DATE("2022-06-01") AND DATE("2022-06-30")
-- 12.82 MB

-- partitioned and clustered:
SELECT DISTINCT(PULocationID) FROM `stoked-champion-410819.week3_zoomcamp.green_partitioned_clustered`
WHERE DATE(lpep_pickup_datetime)
        BETWEEN
          DATE("2022-06-01") AND DATE("2022-06-30")
-- 1.12 MB
```
<a id="q6"></a>

## Question 6: 
Where is the data stored in the External Table you created?

- Big Query
- GCP Bucket
- Big Table
- Container Registry
### Answer: `GCP Bucket`

<a id="q7"></a>

## Question 7:
It is best practice in Big Query to always cluster your data:
- True
- False
### Answer: `False`
 