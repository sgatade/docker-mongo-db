# Docker Compose with MongoDB

## 1. Environment

a. CentOS (AWS Lightsail)
b. Docker : version 18.09.7, build 2d0083d
c. docker-compose : version 1.23.1, build b02f1306
d. Image : [MongoDB:Latest](https://hub.docker.com/_/mongo)

## 2. Pre-requisites
Install Docker, Docker-compose & pull the Mongo image from Docker hub

## 3. docker-compose.yml
This is a very simple version of *docker-compose.yml*. 

3.1. **command:** - Use this to start MongoDB with custom port (other than default 27017).    
3.2. **ports:** - In this example, I'm exposing *4004* port on my *CentOS host*, which will be mapped to port *27072* on my Mongo *Docker instance*. Once done, we will be able to access the MongoDB instance on CentOS using port *4004*. [Note: You need to open this port on your CentOS firewall before trying to connect to the DB from internet.]    
3.3. **volumes:** - *my-mongo-dev-db-data* is a local directory on my *CentOS host*   

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

## 4. Next steps

4.1. Start docker containers...

    docker-compose up

4.2. Get the Mongo docker container id...

    docker ps

4.3. Execute *bash* on the docker container...

    docker exec -t <container_id> bash

#### 5. Get your mongo db ready

5.1. Connect to the mongo instance...

    mongo localhost:27072

5.2. Switch to your database...

    use my-dev-db;

5.3. Create a collection...

    db.createCollection('my-test-table');

5.4. Create the user...

    db.createUser(
        {
            user: "my-user-name",
            pwd: passwordPrompt(),  
            roles: [
                { role: "readWrite", db: "my-dev-db" }
            ]
        }
    )

Note: 

1. You can create and assign multiple *role*s to multiple *db*s
2. You can choose to use a plaintext password instead of a password prompt (but its less secure!)

## 6. Restart the container

6.1. Exit the mongo docker container

    exit

6.2. Shutdown the containers

    docker-compose down

6.3. Edit the *docker-compose.yml*, add *--auth* to the the *command:* section to start mongo with autorisation. The *command:* should look like this now...

    command: mongod --port 27072 --auth

6.4. Save and exit the editor (*wq! if using vi*)

6.5. Start the containers again...

    docker-compose up

**You can now access the mongo db container using the username and password created above.**