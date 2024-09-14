# Docker-beginners

## DockerFile

A docker file is a blueprint that will be used to create a docker image.

## Docker Image

A docker image is a template used to run the container.

## Docker Container

The container is a running process. For example, a node is installed in system A but not in system B. So for running a node in system B without installing using the docker image, we can run a container that will run a node and doesn't get installed directly to system B. 

## Install Tool 

For Windows install docker desktop. With this tool, you can run the container and do operations on a click

1.  Docker ps: its a command for listing out the running containers and images

## Docker File
as its shown in the repo that there is a src/index.js with simple express code to start the server. now we need to run the code but in the system nodejs is not installed. So for running the file and using nodejs create a DOCKERFILE. check the existing DOCKERFILE.

every instruction in dockerfile is considered its own layer, in order to keep things efficeint docker will attempt to cache layers is nothing is actually changed. when youre working on a node project you get your source code then you install dependencies but in docker first install dependencies so they can be cached ,then write a source code

1.  1st line instruction is for installing nodejs 12th version image FORM docker hub. it will give everything to work on node enviroment.

2.  2nd line instruction is for WORKDIR, mention where want to add our app soruce code to the image. 

3.  3rd line is a COPY instruction. here 1st argument is package.json location and 2nd argument is the place we want to copy the file in the container which is a working directory.

4.  4th line is a RUN instruction. now package.json file is present then next install node modules using npm install command.

5.  5th line is for COPY instructions. we dont want to override the node modules that we install so create a file dockerignore and ignore the node_modules so it wont get override.

6.  6th line is for ENV instruction to setting a enviroment for running the file in that enviroment by giving port number. here 8080 port is in use

7.  7th line is for EXPOSE instruction to listening the 8080 port 

8.  8th line is CMD instruction for running the project.

## Build an image

using docker command for building the image first go to the path where docker file is present and open command prompt.
next run the command:

`docker build -t demoapp:1.0 .`

after running this command image will be created successfully with image id and now that we have this image we can use it as a base image to create other images or use it to run the containers. 

## Run a Container

next for running the container use command

`docker run image_id`

check what is the image id via running command 

`docker image ls`

but we cant access the server so for that use below command

`docker run -p 5000:8080 image_id`

-p is for port where server can be accessible on port 5000

## Volumes

there can be situation where want to share data across multiple containers and prefered way to so that is volumes 
a volume is a dedicated folder on the host machine and inside theis folder a conatiner can cretae files that can be remounted into future containers or multiple containers at the same time.
to create a volume we use the docker volume create command

`docker volume create shared-stuff`

shared-stuff is volume name

multiple volume simlutaneously and across the same set of files and the files stick around after all the containers are shut down.

## debugging

if anything is wrong to the container then you can check logs from docker desktop by clicking on the container, there is stats,inspect and logs option to debug the error.

## docker compose

if its a fullstack project with frontend,backend,and database then for those different parts of the project create different containers using docker-compose.yml 

in that version of the project, and services are mentioned.
web services is for node part which is a backend and db services is used via mysql image and for database volume is created.

after writing the yml file next run 

`docker compose up`
or
`docker compose up -d`

### NOTE

for backend dockerfile is created because because image need to be build and for mysql database image is pulled from docker hub. it means if image cant be pulled from docker hub then create docker file, after running docker compose commad it will build the image and run the container.