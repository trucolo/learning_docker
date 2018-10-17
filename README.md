# Learning_docker

## Learning docker from basics

### Downloading an image:

*docker pull image_name:version*

**Search for an image**

*docker search image_name*

** Docker images running **

*docker ps*

** Docker images terminated and running **

*docker ps -a*

**Running an image**

*docker run image_name*  ----> an aleatory name will be given to the image

**Running an image and naming it**

*docker run -d --name=Jenkins jenkins:latest*

**-d** is detached mode, it doesn't attach a terminal to the container

As a side note: Jenkins tries to write on the folder specified with an user whose id is 1000, so in order to run it properly, the right permissions should be given to that folder

*run -d -v /var/jenkins_home:/var/jenkins_home:z -p 8080:8080 -p 50000:50000 --name myjenkins jenkins/jenkins:lts*
z on the command line above is for preserve SE linux permissions.

sudo chown 1000:1000 /var/jenkins_home

on -p parameter, the first port is the one that should be exposed on your host machine and the other is related to the container port, so to turn the jenkins accessible to external world on port 8081, for example, it should be given as

-p 8081:8080

the -v parameter follows the same logic, local folder:container folder

**Inspect the container information**

*docker inspect <container_name>*

**Executing bash application on a given container**

*docker exec -it <CONTAINER_ID|NAME> /bin/bash*

**Check logs on the container**

*docker logs <CONTAINER_ID|NAME>*





