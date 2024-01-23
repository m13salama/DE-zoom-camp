# Module 1: Introduction & Prerequisites

## GOAL:
in this modeule we need to get familiar with docker containers, refresh sql & postgreSQL skills and ...
***note:   
the following tutorial in for linux(ubuntu) mainly***

## 1- Docker:
### what & why docker?
Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run.
I recommend this quick tutorial to get more docker information:
https://www.youtube.com/watch?v=pg19Z8LL06w&pp=ygUGZG9ja2Vy
now why we  need docker as data engineers?
- run data pipelines isolated
- reproducity by using docker images and docker files
- local eperiments
- Integration test(CI/CD)
- Spark
- run pipelines on cload
### Image vs Container??

### build an image and run a container

### docker network
1- create the network using this commmand:   
```docker network create <NETWORK-NAME>```   
2- add these atrributes when you run a container to add the container to a specific network:   
```
--network=<network_name>
--name=<container_name_on_network>
```
### PostgreSQL & PgAdmin
1- create a network:
```
docker network create pg-network
```
2- first we need to run the postgresql container:   
```
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name=pg-database \
postgres:13
```
3- run PgAdmin container:
```
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name=pg-pgadmin \
dpage/pgadmin4
```
### connect PostgreSQL with pgcli