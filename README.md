# Docker Compose with MongoDB

## Environment

1. CentOS (AWS Lightsail)
2. Docker : version 18.09.7, build 2d0083d
3. docker-compose : version 1.23.1, build b02f1306
4. Image : [MongoDB:Latest](https://hub.docker.com/_/mongo)

## Pre-requisites
Install Docker, Docker-compose & pull the Mongo image from Docker hub

## docker-compose.yml
This is a very simple version of *docker-compose.yml*. 

**command:** - Use this to start MongoDB with custom port (other than default 27017).
**ports:** - In this example, I'm exposing *4004* port on my *CentOS host*, which will be mapped to port *27072* on my Mongo *Docker instance*. Once done, we will be able to access the MongoDB instance on CentOS using port *4004*. [Note: You need to open this port on your CentOS firewall before trying to connect to the DB from internet.]
**volumes:** - *my-mongo-dev-db-data* is a local directory on my *CentOS host*

    version: '3'

    services:

        my-mongo-dev:
            container_name: my-mongo-dev
            image: mongo:latest
            command: mongod --port 27072
            ports:
                - 4004:27072
            volumes:
                - /my-mongo-dev-deb-data:/data/db

## Manual Steps 1

## Get into docker container
docker ps -> Copy container id
docker exec -t <container_id> bash

#### Create a collection
mongo localhost:27007
db.createCollection('testCollection');

#### Create user manually
db.createUser(
  {
    user: "my-user-name",
    pwd: passwordPrompt(),  // or cleartext password
    roles: [
       { role: "readWrite", db: "my-db" }
    ]
  }
)

## Manual Steps 2
Restart docker-compose with mongo --auth