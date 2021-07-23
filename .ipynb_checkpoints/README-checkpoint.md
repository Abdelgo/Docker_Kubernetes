### DOCKER & Kubernetes ######

what is Docker ? 

why Docker ? 

Docker Image

Container ? 


# Docker CLI Commands
docker Version
docker ps
docker ps --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
ae0ffd345513        busybox             "ping google.com"   2 minutes ago       Exited (0) 52 seconds ago                       festive_tu

docker run "container Image ID"

Docker RUN = Docker create + docker start

Restarting a stopped container

docker start "Container Id" (no argument given because the container ID has already the 'command')

docker start -a "Container Id" (-a to see the output) 

## Remove containers
docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache
  
to see logs of a running container
docker logs "Container Id"

to stop a container 

Docker stop "Container Id" (send a SIGTERM signal saying stop the process when you are ready 'smooth stop')
§ docker stop will be executed for 10s, if container not stopped then docker sill send a kill signal" $ 

docker kill "Container Id" (send a SIGKILL signal saying stop now, no additional work executed!)

## running a command inside a running container
docker exec -it "container ID" "Command"
docker exec -i -t "container ID" "Command" (the -i is for the Stdin input in linux and the t is for the nice format)
ex: 
docker exec -it "container ID" redis-cli
127.0.0.1:6379> set myvalue 5
OK
127.0.0.1:6379> get myvalue
"5"
127.0.0.1:6379> 

## Start a shell
docker exec -it "container ID" sh (to run a shell command page inside the docker container)  
it is also possible the run the container with a start of shell at the same time with the following command:  
docker run -it "container ID" sh

## Container Isolation:
we build 2 Containers from the same Docker Image, thoses 2 containers don't share the same 'file system'
ex : 


