# Use an existing docker image as a base
# update the base image (that include npm) - node with alpine tag mean the most light node base
FROM node:alpine

#Specifiying the working directory where the app files will be copied
WORKDIR /usr/Node_js_app

#Download and install dependency
# COPY (the build context- source directory where the requirement file) (target directory where to install)
COPY ./ ./
RUN npm install

#tell the image what to do when it starts as a container
CMD ["npm" , "start"]

#update from the abdelok contributor