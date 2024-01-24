## Module 1 Homework
the homework link is [here](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/cohorts/2024/01-docker-terraform/homework.md)

# Table of Contents
- [Question 1](#q1)
- [Question 2](#q2)
- [prepare postegreSQL](#p)
- [Question 3](#q3)
- [Question 4](#q4)
- [Question 5](#q5)
- [Question 6](#q6)
- [Question 7](#q7)

<a id="q1"></a>
## Question 1. Knowing docker tags

***Answer***   
--rm

***Steps***   
1- open terminal   
2- run "docker run --help" command   
3- search for the target text   
![results](./screenshots/run--help.png)


<a id="q2"></a>
## Question 2. Understanding docker first run 

***Answer***   
0.42.0

***Steps***   
1- open terminal   
2- run following command:```docker run -it --entrypoint="bash" python:3.9```   
3- in the bash run: ```pip list```  
![results](./screenshots/pip-list.png)

<a id="p"></a>
# Prepare Postgres

***steps***   
0- download datasets using wget:   
```wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-09.csv.gz```   
```wget https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv```   
1- build the dockerfile ```docker build -t taxi_ingest:v001```   
2- run the docker-compose ```docker-compose up```
3- run a python server ```python -m http.server```
4- run the ingest container to ingest taxi data
```
URL="http://<your-ip>:8000/green_tripdata_2019-09.csv.gz"
docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi_green \
    --table_name=green_taxi_trips \
    --url=${URL}
```   
5- run the ingest container to ingest zone data
```
URL="http://<your-ip>:8000/taxi%2B_zone_lookup.csv"
docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi_green \
    --table_name=zones \
    --url=${URL}
```  
<a id="q3"></a>
## Question 3. Count records 

***Answer***   
15612

***Steps***   
1- open PgAdmin   
2- run this query 
```
SELECT COUNT(*)
FROM green_taxi_trips
WHERE DATE(lpep_pickup_datetime) = '2019-09-18' AND DATE(lpep_dropoff_datetime) = '2019-09-18';

```   
![results](./screenshots/18th.png)


<a id="q4"></a>
## Question 4. Largest trip for each day

***Answer***   
2019-09-26

***Steps***   
1- open PgAdmin   
2- run this query 
```
SELECT DATE(lpep_pickup_datetime), sum(trip_distance)
FROM green_taxi_trips
group by 1
order by 2 desc
limit 1;

```   
![results](./screenshots/distance.png)

<a id="q5"></a>
## Question 5. Three biggest pick up Boroughs
***Answer***   
"Brooklyn" "Manhattan" "Queens"

***Steps***  
1- open PgAdmin   
2- run this query 
```
select z.borough, sum(t.total_amount)
from green_taxi_trips t
join zones z
on t.pu_location_id = z.location_id
where DATE(lpep_pickup_datetime) = '2019-09-18' and z.borough != 'Unknown'
group by 1
having sum(t.total_amount) > 50000
order by 2 desc
limit 3;
```
![results](./screenshots/borough.png)

<a id="q6"></a>
## Question 6. Largest tip

For the passengers picked up in September 2019 in the zone name Astoria which was the drop off zone that had the largest tip?
We want the name of the zone, not the id.

Note: it's not a typo, it's `tip` , not `trip`

- Central Park
- Jamaica
- JFK Airport
- Long Island City/Queens Plaza

***Steps***  
1- open PgAdmin   
2- run this query   
```
select z_do.zone, sum(t.tip_amount)
from green_taxi_trips t
join zones z_pu
on t.pu_location_id = z_pu.location_id
join zones z_do
on t.do_location_id = z_do.location_id
WHERE EXTRACT(YEAR FROM lpep_pickup_datetime) = 2019 
  	AND EXTRACT(MONTH FROM lpep_pickup_datetime) = 9
	AND z_pu.zone = 'Astoria'
group by 1
order by 2 desc
limit 100;
```

## Terraform

In this section homework we'll prepare the environment by creating resources in GCP with Terraform.

In your VM on GCP/Laptop/GitHub Codespace install Terraform. 
Copy the files from the course repo
[here](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main/01-docker-terraform/1_terraform_gcp/terraform) to your VM/Laptop/GitHub Codespace.

Modify the files as necessary to create a GCP Bucket and Big Query Dataset.

<a id="q7"></a>
## Question 7. Creating Resources

After updating the main.tf and variable.tf files run:

```
terraform apply
```

Paste the output of this command into the homework submission form.
