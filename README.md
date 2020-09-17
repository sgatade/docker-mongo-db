# Docker Compose with MongoDB

## Environment

1. CentOS (AWS Lightsail)
2. Docker : version 18.09.7, build 2d0083d
3. docker-compose : version 1.23.1, build b02f1306
4. Image : [MongoDB:Latest](https://hub.docker.com/_/mongo)

## Pre-requisites
Install Docker, Docker-compose & pull the Mongo image from Docker hub

## docker-compose.yml

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