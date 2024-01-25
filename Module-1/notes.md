# Module 1: Introduction & Prerequisites
# Table of Contents
1. [Docker](#1-docker)
    - [What & Why](#what&why)
    - [Image vs Container](#image&container)
    - [Volumes](#volumes)
    - [build & run](#build&run)
    - [Network](#network)
    - [Docker-compose](#compose)

2. [PostgreSQL](#postgresql)

3. [Access PostgreSQL](#access)
    - [Using PgAdmin](#pgadmin)
    - [Using Pgcli](#pgcli)
    - [Using python](#python)

4. [TerraForm](#terra)

## 1-Docker

<a id="what&why"></a>
### what & why docker
Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime, so now we can solve "it works on my machine" problem.
I recommend this quick tutorial to get more docker information:
https://www.youtube.com/watch?v=pg19Z8LL06w&pp=ygUGZG9ja2Vy   
now why we  need docker as data engineers?
- run data pipelines isolated
- reproducity by using docker images and docker files
- local eperiments
- Integration test(CI/CD)
- Spark
- run pipelines on cload

<a id="image&container"></a>
### Image vs Container??
1- Docker image:   
It is a snapshot or template that contains the application and its dependencies, we often writes Dockerfile that contains instructions to build our Image   
for example: that's the dockerfile for running a python script on base Image python:3.9([here](Dockerfile))   
2- Docker container:
A container is a running instance of an image.   
remember that containers are isolated and ephemeral.

<a id="volumes"></a>
### Volumes
still remeber that containers are ephemeral.   
so what should you do to get persistent storage??   
the answer is the volumes:
volumes are directories or files that are mounted from the host machine or other containers.   
Volumes allow for persistent storage and data sharing between the container and the host or between containers.
example to associate volume to a container:
```
docker run -it -v <host-path>:<container-path> Image:tag
```

<a id="build&run"></a>
### build an image and run a container
images are stored in dockerhub locally or remote.   
***to build a new image:***   
1- make dockerfile as [here](Dockerfile)   
2- build the docker using:   
```
docker build -t <Image>:<tag> <path/of/dockerfile>
```   
***to run an Image:***
```
docker run -it <Image>:<tag>
```

<a id="network"></a>
### docker network
do you still remebering the containers are isolated?   
so what to do if you wannna make 2 containers communicate?   
the answer is create a network then make the container communicate through it like that:   
***1- create the network using this commmand:***   
```
docker network create <NETWORK-NAME>
```   
***2- add these atrributes when you run a container to add the container to a specific network:***   
```
--network=<network_name>
--name=<container_name_on_network>
```

<a id="compose"></a>
### docker compose
Docker Compose is a tool for defining and running multi-container applications. It is the key to unlocking a streamlined and efficient development and deployment experience.   
1- install docker-compose   
2- write configuration file(.yaml) as [here](docker-compose.yaml)   
3- run
```
docker-compose up
```   
<a id="postgresql"></a>
## 2- PostgreSQL:

In this section we want to run postgresql container then run a python script on another container to ingest data from a csv file to the database.

### 
1- create a network:   
let's create the network where the containers would communicate.
```
docker network create pg-network
```
2- first we need to run the postgresql container:   
use this command to run the container and mount it to a volume in addition to running it on a specific network.   
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
3- run http-server using python:   
you can run this simple http server to access local files via the network instead of downloading them in the containers.
```
python3 -m http.server
```
4- run Ingest Script:   
if you have your script in a jupyter notebook use this command to convert the notebook to a script.
- convert jupyter notebook to script:
```
jupyter nbconvert --to=script <notebook-name>
```
5- build and run docker image:
- build the container using dockerfile [here](Dockerfile) and this command ```docker build -t taxi_ingest:v001 .```
- run the container using:
```
URL="http://<your-ip>:8000/yellow_tripdata_2021-01.csv"
docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```
- now if the script [here](ingest_data.py) finish correctly the database is loaded with our dataset.

<a id="access"></a>
## 3- Access PostgreSQL:
now we have our database running, but w need to access it we can do so by many methods as following:

<a id="pgadmin"></a>
### Using [PgAdmin](https://www.pgadmin.org/):
pgAdmin is the most popular and feature rich Open Source administration and development platform for PostgreSQL.

1- run PgAdmin container:   
use this command to run the container on specific network (same network of postgresql container)
```
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name=pg-pgadmin \
dpage/pgadmin4
```
2- open port 8080 on any browser and sign in your database.

<a id="pgcli"></a>
### Using [pgcli](https://www.pgcli.com/):
Pgcli is a command line interface for Postgres with auto-completion and syntax highlighting.   
run this command to open the cli: ```pgcli -h localhost -p 5432 -u root -d ny_taxi```

<a id="python"></a>
### Using python:
use alchemy library to connect to the database as [here](pg-test-connection.ipynb)


<a id="terra"></a>
## 4- TerraForm:

<a id="what-terra"></a>
### what is terraform ??
it's just Infrastructure as code.

### key commands:
- init: initializes a new or existing Terraform configuration, downloading necessary provider plugins and setting up the working directory.
- plan: generates an execution plan, outlining the changes Terraform will make to achieve the desired infrastructure state, without actually applying the changes.
- apply: executes the planned changes, applying the Terraform configuration to create, modify, or destroy infrastructure resources as specified in the configuration files.
- destroy: used to tear down and destroy all resources created by the Terraform configuration, helping to clean up the infrastructure when it is no longer needed.

### Example:
[here](./terra)
