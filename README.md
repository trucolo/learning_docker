# Learning_docker

In order to have access to docker deamon, the user must be part of group docker, it can be achieved by:

sudo usermod <username> -G docker  --- It should be avoided on production environments though.

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

*docker run -d -v /var/jenkins_home:/var/jenkins_home:z -p 8080:8080 -p 50000:50000 --name myjenkins jenkins/jenkins:lts*
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

**Restarting an image**

*docker restart <CONTAINER_ID|NAME>*

**Removing an image** - Note that it won't remove images that have a container attached to them, you must remove the container first and then remove the image, unless -f is used to force the removal.

*docker rmi image_name*

**Filtering images attached to some base image**

*docker ps -a -q -f "ancestor=<base image name>"*

Example:
docker ps -a -q -f "ancestor=hello-world"

There are multiple parameters that can be used to filter containers, for reference: https://docs.docker.com/engine/reference/commandline/ps/#filtering

**Exposing ports when running images**

*docker run -d --name=WenServer1 -P nginx:latest*   -P exposes a random port on the host machine.

To check the ports mapped, you can run:
*docker ps*
*docker port <CONTAINER_NAME> $CONTAINERPORT*

To assign a specific port
*docker run -d -p 8080:80 --name=WebServer1 nginx:latest*   Being 8080 the port of host machine and 80 the container port

**Mapping directories between host and container**

*docker run -d -p 8080:80 --name=WebServer1 **-v /home/user/www:/usr/share/nginx/html** nginx:latest
-v <local dir>:<container dir>
  
**Making changes to a container and creating a new image from that**

After making some change, for example, installing a given package on a container, the base image won't be affected, however, if you want that change to be permanent to that container, and create a new image with that change, you can by doing:

*docker commit -m "Comment" -a "Author" <name_of_the_container> <name_of_the_new_image>:<version of the image>*












