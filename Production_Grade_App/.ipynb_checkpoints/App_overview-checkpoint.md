# Project Overview

for this project we will use a production grade workflow as described below:  
<img src="photos/1.png" >
<img src="photos/2.png" >

## Aside view about the react app*
<img src="photos/3.png" >

generate the react app bootstrap example, we type the following command:  
**npx create-react-app frontend**

## DOCKERFILE
as we have different environment (development and production), so it's normal to have 2 dockerfiles.
<img src="photos/4.png" >
- Dockerfile.dev
- Dockerfile.prod

each dockerfile will be used to create the docker image for (development and production)
## building the dev image
**docker build -f Dockerfile.dev . (-f to specify the file we want to use to build the image)

## Using Docker Volumes
this option is very helpful when we want to update our app images directly without rebuilding the project, the trick is instead of copying the app files inside the container (COPY . .) we will open like a port mapping to permit to the container to get those files from our local directory.  
<img src="photos/5.png" >
the bookmark is the file that is inside the container that we don't want to be part of the mapping to our local directory  
<img src="photos/6.png" >

if there is no conflict or you don't want to remove a specific folder/or file mapping you can go with :  
docker run -d -p 3000:3000 -v $(pwd):/app ImageID

$
## Using docker volumes with Docker-compose
to simplify the long running command above, we can use the docker-compose to simplify
<img src="photos/7.png" >