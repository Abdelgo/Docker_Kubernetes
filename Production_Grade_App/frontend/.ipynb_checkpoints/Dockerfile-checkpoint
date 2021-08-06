FROM node:alpine

WORKDIR /app

COPY package.json .
RUN npm install

COPY . . 

RUN npm run build

FROM nginx
# --from=0 means from where step 0 finished we take a copy of /app/build
COPY --from=0 /app/build /usr/share/nginx/html 

#there is no CMD for nginx because its done automatically
# by default nginx serve port 80