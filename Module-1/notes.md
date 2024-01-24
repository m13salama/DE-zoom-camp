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

3. [TerraForm](#section-2)

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
2- write configuration file(.yaml)   
3- run
```
docker-compose up
```   
<a id="postgresql"></a>
## 2- PostgreSQL:

in this section we want to run postgresql and pgadmin containers and make them in thesame network to communicate then run a python script on another container to ingest data from a csv file to the database.

### 
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
4- run Ingest Script:
- convert jupyter notebook to script:
```
jupyter nbconvert --to=script <notebook-name>
```
- run http-server using python:
```
python3 -m http-server
```
- build and run docker image:

### connect PostgreSQL with pgcli

## 3- TerraData:
