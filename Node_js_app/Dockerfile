# Use an existing docker image as a base
# update the base image (that include npm) - node with alpine tag mean the most light node base
FROM node:alpine

#Specifiying the working directory where the app files will be copied
WORKDIR /usr/Node_js_app

#Download and install dependency
# COPY (the build context- source directory where the requirement file) (target directory where to install)
# in order to avoid reinstallation in case of changing the files other than installation requirements, we take only the package.json and copy it inside the workdir
COPY ./package.json ./
RUN npm install

# Then we can copy all the files that are included in our app inside the workdir
COPY ./ ./
#tell the image what to do when it starts as a container
CMD ["npm" , "start"]

#update from the abdelok contributor
