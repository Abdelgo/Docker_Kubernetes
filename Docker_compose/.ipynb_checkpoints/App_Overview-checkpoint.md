# Project: create an application with database

## 1 Architecture overview : 
the project is to make an app that show the number of visits, this app contains:  
- the app
- the database

Solution 1: put the app and the database in the same Image, but this solution is not scalable in case we would use multi containers (the database will have different values from container to another)  
Solution 2: create an app Image and another one with the database (Redis here)

> DOCKER CONTAINER(Node App)
> DOCKER CONTAINER(Node App) <------>  DOCKER CONTAINER(Redis)
> DOCKER CONTAINER(Node App)

the communication between the both containers will be assured by Docker Compose

## Introducing Docker Compose
- Separate CLI that gets installed along with Docker
- Used to start up multiple docker containers at the same time
- automates some of the long-winded arguments we were passing to "docker run"

## YAML FILE description
**docker-compose.yml**
here are the containers I want to create  
- redis-server
    - make using the redis Image
- Node_app
    - Make it using the docker file in the current directory
    - map port 4001 to 8081
    
**Remark** the docker-compose.yaml will create the both containers on the same network, so no network mapping to do in between the node_app and the redis-server  
<img src="/photos/1.png" >

the port mapping is only done for the mapping between outide of containers to the inside containers