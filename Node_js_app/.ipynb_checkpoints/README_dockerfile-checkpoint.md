# Dockerfile for Node js App details
<img src="./photos/6.png">
1) specify the base
FROM node:alpine  
**remark** node:alpine (alpine stays for the modt light version of node)
2) Specifiying the working directory where the app files will be copied
WORKDIR /usr/Node_js_app

3) Download and install dependency (additional programs)
before to install the dependencies we need to specify what are dependencies and where it should be installed, so we use COPY command
<img src="./photos/7.png">
<img src="./photos/8.png">
RUN npm install
> to install other dependencies like packages included in the files attached "packages.json" we need to specify WORKDIR
Node_js-app:
    - README.md
    - Dockerfile
    - package.json
    - index.js  
    
4) tell the image what to do when it starts as a container
CMD ["npm" , "start"]
