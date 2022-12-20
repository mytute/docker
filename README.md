# docker

## docker manual tutorial

resource
1. https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
2. https://www.docker.com/blog/getting-started-with-docker-using-node-jspart-i/

## install with ubuntu

* install and check version.

```bash
$ sudo apt install docker.io
$ docker -v
```

* start docker    

```bash
# check whether is docker active or inactive.
sudo systemctl status docker

# if Active:inactive so you can enable using following cmd
sudo systemctl enable --now docker
```

* check is docker is working find(start docker project call 'hello-world')      

because of we don't have docker image call "hello-word" docker is import that image from docker remote repo and start to work here.

```bash
$ sudo docker run hello-world    
```
* view all docker images on machine      

```bash
$ docker images
```

for more info >> https://docs.docker.com/engine/install/ubuntu/

## docker with node app


<!-- build docker image

docker build -t express .

run docker application
docker run -p 3000:3000 express   -->


Creating a Dockerfile

```bash
$ touch Dockerfile
```

```bash
# define node version
FROM node:12

# create app directory
WORKDIR /Desktop/docker

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source inside docker image
COPY . .


# set port to run this docker image
EXPOSE 8080

# cmd to start node server
# Eg:
# $ node server.js
CMD [ "node", "server.js" ]
```

Add .dockerignore file. (add to same directory)    
this for prevent node_modules and logs not to copy to docker image.      

```bash
node_modules
npm-debug.log
```

Build docker image  (this care about use npm version: latest is best)
```bash
$ sudo docker build . -t <your_username>/node-web-app
```
Build docker image
```bash
$ docker build --tag <node_image_tag_name> .
```

Run docker image( test fail without publish )  
```bash
$ sudo docker run <your docker image tag>
```

Stop the docker image   
first you need to open new terminal and find the target container id    

```bash   
$ docker ps # get the id of the running container
$ docker stop <CONTAINER ID> # kill it (gracefully)

```

To publish on to local network    

1.first you should stop docker container.            
2.next make run angain with following additional tags.  

 —publish [host port]:[container port].      

```bash
$ sudo docker run --publish 8000:8000 <your_docker_image_tag>
```

Test docker image    
```bash
$ curl --request POST \
  --url http://0.0.0.0:8000/test \
  --header 'content-type: application/json' \
  --data '{ "msg": "some_data"}'
```

https://github.com/alpinelinux/docker-alpine

## docker alpine tutorial
https://hub.docker.com/r/mhart/alpine-node


## common

Remove docker builded image

```bash
$ sudo docker image rm [OPTIONS] IMAGE [IMAGE...]
```

# docker with Angular 

create "Docker.dev" and  "Docker" files on algular root directory for create docker for development and production. 

create ".dockerignore" file for not to copy node modules to docker container by copy all ".gitignore" code to ".dockerignore" file. 

> "Docker.dev"

```bash
FROM node:16.17.1

#define working derectory 
ARG WORK_DIR=/frontend 
ENV PATH ${WORK_DIR}/node_modules/.bin:$PATH

# create working derectory 
RUN mkdir ${WORK_DIR}
WORKDIR ${WORK_DIR}

# copy files to docker container from repo
COPY package.json ${WORK_DIR}
COPY package-lock.json ${WORK_DIR}

RUN npm install @angular/cli@14.2.10
RUN npm install

COPY . ${WORK_DIR}

EXPOSE 4200

CMD ng serve --host 0.0.0.0
```





