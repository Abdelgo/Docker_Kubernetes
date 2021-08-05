### DOCKER & Kubernetes ######

what is Docker ? 

why Docker ? 

Docker Image

Container ? 


# Docker CLI Commands
https://docs.docker.com/engine/reference/commandline/image_rm/ (link to docker CLI)    
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

## remove docker image
docker image rm -f 4a6ddcc1d5db

## Remove containers  
docker system prune  
WARNING! This will remove:  
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache
  
# to see logs of a running container  
docker logs "Container Id"  

# to stop a container  

Docker stop "Container Id" (send a SIGTERM signal saying stop the process when you are ready 'smooth stop')  
docker stop will be executed for 10s, if container not stopped then docker sill send a kill signal" 

docker kill "Container Id" (send a SIGKILL signal saying stop now, no additional work executed!)  

# running a command inside an already running container 
docker exec -it "container ID" "Command"  
docker exec -i -t "container ID" "Command" (the -i is for the Stdin input in linux and the t is for the nice format)  
ex:   
docker exec -it "container ID" redis-cli  
127.0.0.1:6379> set myvalue 5  
OK  
127.0.0.1:6379> get myvalue  
"5"  
127.0.0.1:6379>   

another example with a shell start :    
docker exec -it "container ID" sh (to run a shell command page inside the docker container)   
it is also possible the run the container with a start of shell at the same time with the following command:   
docker run -it "container ID" sh

# running the docker container in the background
docker run imageId -d


## Container Isolation:  
we build 2 Containers from the same Docker Image, thoses 2 containers don't share the same 'file system'  
ex :  


# building the docker Image

## a- specifing a name (to be used instead of "container ID")

<img src="./photos/5.png">

Docker build -t yourdockerID/Repo(projectname):version  

## b- creating manually a docker image with docker commit
1) create the base  
docker run -it Alpine sh 'this will open the shell using the alpine base'

2) Install dependencies inside the alpine shell
apk add --update redis

3) Open another terminal page
docker ps (and look for the running container) to get the container ID. 
docker commit -c 'CMD ["redis-server"]' CONTAINERID  
A new image ID will be created

docker run "image ID"

# PORT Mapping in docker :
the port mapping is done at the run time using the folowing command :  
docker run -p 8080:8080 ImageId
<img src="/photos/9.png">
<img src="/photos/10.png">

# Specifying the Working Directory:
in order to avoid any mess with base image files included in the directory of your container, for example you could have a conflict between a file name var in your application and the the one in the container base directory. This is why it's very important to sepcifiy a WORKDIR to avoid this kind of problems that can override some important files.  

<img src="/photos/11.png">

see below example (WORKDIR /Node_js_app) :  
livai$ docker exec -it 13e59d931553  sh
/Node_js_app # ls
Dockerfile         README.md          index.js           node_modules       package-lock.json  package.json
/Node_js_app # cd ..
/ # ls
Node_js_app  dev          home         media        opt          root         sbin         sys          usr
bin          etc          lib          mnt          proc         run          srv          tmp          var
