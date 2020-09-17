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

## Next steps

Start docker containers...

    docker-compose up

Get the Mongo docker container id...

    docker ps

Execute *bash* on the docker container...

    docker exec -t <container_id> bash

#### Get your mongo db ready

Connect to the mongo instance...

    mongo localhost:27072

Switch to your database...

    use my-dev-db;

Create a collection...

    db.createCollection('my-test-table');

Create the user...

    db.createUser(
        {
            user: "my-user-name",
            pwd: passwordPrompt(),  
            roles: [
                { role: "readWrite", db: "my-db" }
            ]
        }
    )

Note: 

1. You can create and assign multiple *role*s to multiple *db*s
2. You can choose to use a plaintext password instead of a password prompt (but its less secure!)

## Restart the container

Exit the mongo docker container

    exit

Shutdown the containers

    docker-compose down

Edit the *docker-compose.yml*, add *--auth* to the the *command:* section to start mongo with autorisation. The *command:* should look like this now...

    command: mongod --port 27072 --auth

Save and exit the editor (*wq! if using vi*)

Start the containers again...

    docker-compose up

You can now access the mongo db container using the username and password created above.