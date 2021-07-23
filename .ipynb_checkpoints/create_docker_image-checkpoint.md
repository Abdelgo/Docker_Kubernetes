# Building docker Images

<img src="./photos/1.png">

<img src="./photos/2.png">

# Activity: create a docker image that runs redis-server

1) create a Dockerfile
dockerfile:
"Use an existing docker image as a base"
FROM alpine "this is the base image"

"Download and install dependency"
RUN apk add --update redis  "the command to be run"

"tell the image what to do when it starts as a container"
CMD ["redis-server"] "redis-server is the argument to CMD command"

2) Build the image

> cd "file where the you have the dockerfile"
> docker build .

# creating Docker image analogy
<img src="./photos/2.png">