# APP Overview
<img src="photos/1.png">

## Databases Connections
<img src="photos/2.png">

## APP workflow
<img src="photos/3.png">

## Nginx request routing
### the problem
how to trig the correct for the correct request, here the role of Nginx to organize this:  
<img src="photos/4.png">

Nginx will do the routing using the target link:
<img src="photos/5.png">  

the routing is done on the file default.conf included inside the Nginx server :  
<img src="photos/5.png">

finaly the docker-compose file is as below :   
```YAML
version: '3'
services:
  postgres:
    image: 'postgres:latest'
    environment:
      - POSTGRES_PASSWORD=postgres_password # by default password 
  redis:
    image: 'redis:latest' 
  nginx:
    depends_on:
      - api  # to avoid errors while connecting to upstream
      - client
    restart: always
    build:
      dockerfile: Dockerfile.dev # dockerfile to be used
      context: ./nginx # the folder where you have the dockerfile
    ports:
      - '3050:80' # routing to expose localhost:3050 to the nginix by default port 80 (in the Nginix container)
  api:
    build:
      dockerfile: Dockerfile.dev
      context: ./server
    volumes:
      - /app/node_modules
      - ./server:/app
    environment:
      - REDIS_HOST=redis  # DB environment variables 
      - REDIS_PORT=6379
      - PGUSER=postgres
      - PGHOST=postgres
      - PGDATABASE=postgres
      - PGPASSWORD=postgres_password
      - PGPORT=5432
  client:
    stdin_open: true
    build:
      dockerfile: Dockerfile.dev
      context: ./client
    volumes:
      - /app/node_modules
      - ./client:/app
  worker:
    build:
      dockerfile: Dockerfile.dev
      context: ./worker
    volumes:
      - /app/node_modules
      - ./worker:/app
    environment:
      - REDIS_HOST=redis # Redis environment variables used to get or to put values from server(api)
      - REDIS_PORT=6379
```