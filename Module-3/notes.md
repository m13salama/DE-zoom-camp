# Warehousing

## OLTP vs OLAP

## what is a data warehouse ?
<img src="./images/">
- Olap solution used for reporting and data analysis
- many data sources staged together then stored in the warehouse
- warehouse = meta data + raw data + summary data
- you can get data from ware house directly or through a data mart

## what is BigQuery
- cloud based warehouse from GCP
- serverless --> google take care of that
- built in features like:
    - machine learning
    - geospatial analysis
    - business intelligence
- costing
    - on demand --> 1TB = 5$
    - Flat rate --> based on slots --> 100 slots = 2000$/month
- partitioning
    - read only needed partitions = efficiency
    - bias should be avoided
    - time unit or integer range
    - max(partitions) = 4000
- clustering
    - colocate related data
    - like secondary indexing
    - clustering improve filtering and aggregate
    - order of clustering columns is important
    - up to 4 clusters
    - data < 1GB --> no significant effect
    - automatic reclustering
- BQ best practice
    - avoid select * --> use columns names
    - price your queries before running
    - use clustering and partitioning
    - using streaming inserts with caution
    - materialize query results in stages
    
<img src="./images/">

## BigQuery Internals

## BigQuery & machine learning