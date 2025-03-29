# 05-Docker

Old enterprise vs Monilithic vs Micro services

Old enterprise - everything will be together, Both front end, backend will be in same application. If any changes in frontend/backend, entire application need to be re-deployed.

Monolithinc - In this both frontend and backend are seperately deployed. And they connected through API. If the backend has multiple 

services/modules (user,payment,shipping, delivery, cart, product, customer support, reviews etc), if these is a change in one module then all the modules need to be deployed (full deployment) in backend.

Microservices - every service/module is seperated in microservices. Each module is connected together through APIs. Each service/module connects to other module through APIs and get the data.

Each service can be individually deployed.

In old type servers, servers are not fully utilized. For eg: if we have a server of 100GB ram for an application, we may not utilise it completely. We may use aroung 30GB or 40 GB.

One server is dedicated to use one OS only. It blocks the resources.

But in Containers we use the resources fullly. Containers do not block resources. 

![image](https://github.com/user-attachments/assets/5197ca1e-32d8-4b20-b7e2-6a6343ea755b)

Docker Image -> Bare minimum OS (very less size of around 10 MB only) + we configured (installation, code, dependencies, packages, services ect)

Installind Docker:

Launch an EC2 from aws

https://docs.docker.com/engine/install/rhel/

Install the dnf-plugins-core package (which provides the commands to manage your DNF repositories) and set up the repository.
```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```
Install Docker Engine

Install the Docker packages.
```
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Start Docker Engine.
```
sudo systemctl enable --now docker
```
To see the staus of docker
```
sudo systemctl status docker
```

When docker is installed, agroup called 'docker' is created. To allow the normal user 'ec2-user' to run docker commands, we need to add ec2-user to docker group

```
sudo usermod -aG docker docker ec2-user
```
exit and login again to get the effect

Docker Commands:

Docker image is a static file. When we run that image , it is a container. Container is the running instance of image. VM is the running instance of AMI.

To check the running containers
```
docker ps
```
To display the images available in server
```
docker images
```
To pull the image from docker repository/hub. Docker hub is similar to github, where the official images exist eg:nginx
```
docker pull <image-name>
```

Multiple users can push the images to docker hub. To get the uniqueness, docker made the naming convention as follows

username/image-name:version

eg:

suresh/nginx:1.0.0

suresh/nginx:1.0.1

Official images naming as follows:

image-name:version

or 

image-name:tag

eg:

nginx:1.0.0

nginx:alpine-slim

To pull the latest version

```
docker pull nginx:latest
docker images
```
or by default it pulls the latest

```
docker pull nginx
docker images
```

To create the container from docker image
```
docker create <image-name>
or
docker create <image-id>
docker create nginx:alpine-slim
```

To see all the containers 
```
docker ps -a
```

To see running containers
```
docker ps
```
TO run a container
```
docker ps -a
docker start <container-id>
docker ps
```

TO stop a container

```
docker stop <container-id>
```
To remove a container
```
docker rm <container-id>
```
TO force remove a running container
```
docker rm -f <container-id>
```

To delete docker images in the server
```
docker rmi <image-id>
```
Pulling the image, creating container and running container we need to use 3 commands. We can use docker run command directly to do all these 3 tasks. docker run command pulls the image, creates the container and starts the container.
```
docker run <image-name>
```
The above command runs and creates/runs the container in the foreground.

To run in the background use -d  (detaching from screen)
```
docker run -d <image-name>
```

Container is like a mini/nano server. It also has ports ranging from 0 to 65535 ports (container ports).

Server has its own ports (host ports). For eg if we run an nginx container in the server, container is having port 80. 

We need to map host port with container port. We can use any host portnumber to map with container port number.

-p <host-port>:<container-port>

```
docker run -d -p <hostport:container-port> <image-name>
docker run -d -p 80:80 nginx
```
After mapping the port by running above command, if we give IP address in the url, it shows nginx website.

We can run many containers in the server by using same image also. We can run multiple nginx containers in one server also, as below
```
docker run -d -p 8080:80 nginx
docker run -d -p 8081:80 nginx
```
Port mapping is called as port forwarding

TO go inside the container the command is
```
docker exec -it <container-id> bash
```
it is interactive terminal. bash is bash terminal

To get the full information of about container
```
docker inspect <container-id>
```
TO see the docker logs
```
docker logs <container-id/container-name>
```
When we create/run a contianer it give a random name to the container by clubbing two words with underscore symbol.

If we want give a name to container

```
docker run -d -p 8082:80 --name suresh nginx
```

DOCKER FILE:

docker file is used to build custom images. We can make use of docker instructions to create custom images.

Docker file is the declarative way of creating docker images using docker instructions.

Follow the docker reference link for docker instructions  https://docs.docker.com/reference/dockerfile/

Custom image is just like slecting a base OS, installing applocation runtime, place the code and create the image.

some docker instructions are 

FROM RUN CMD ENTRYPOINT COPY ADD ENV ARG USER WORKDIR 

FROM:

FROM should be the first instruction in Dockerfile. It represents the base OS. There is an exeption ARG. Mostly(99%) FROM is first instruction.

In VMs are using RHEL OS, its of very high memory. RHEL also provides docker image which is of light weight. 

Any docker file which we are writing, its file name should be Dockerfile.

TO Biuld a docker image:

We need to write a Dockerfile amd give below docker build command to create a docker image. Dockerfile should present in the current directory before running the below build command
```
docker build -t <image-name>:<version> .
```
Here we can give our own image name and version. '.' indicates current directory. t is tagging the image

RUN:

RUN instruction is used to configure the image with packages and other configurations. RUN instruction runs at the time of image building

CMD:

CMD instruction runs at the time of container creation. (i.e. when we run docker run command, CMD instruction runs)It keeps container running.

systemctl commands work for VMs. It does not work for containers. (For eg: systemctl start nginx) We need to use CMD to run nginx

To run nginx in foreground
```
CMD ["nginx", "-g". "daemon off;"]
```
daemon-off is to run the command in foreground

There can only be one CMD instruction in a Dockerfile. If you list more than one CMD, only the last one takes effect.

To change the tag of a docker image:
```
docker tag <old tag> <new tag>
docker tag suresh-image-name:v1 joindevops/nginx:v1
```

To push a docker image to docker hub, we need to login to docker hub and then push the image
```
docker login -u <username>
docker push <image-name:version>
docker push joindevops/nginx:v1
```
CMD vs RUN difference in dockerfile:

CMD runs at the time of container creation. RUN command runs at the time of image creation

LABEL:

LABEL instruction adds meta data to image. Like a key value pair. Like description of image, owner name, project name. Labels are used to filter the images

```
LABEL author="suresh" \
      company = "ibm" \
      topic="docker files"
```

This is useful especially, when we need to filter and list down the existing images.
eg:
```
docker images -f "label=company=ibm"
```
It lists the label having company name is ibm

EXPOSE:

is used to know the list of ports opened by the conatiner, when the container runs. EXPOSE instruction does not effect functionality. It only gives info about ports.
The EXPOSE instruction doesn't actually publish the port. User knows the exposed ports of an image by using docker inspect command. 
In the docker file, image builder person specifes the ports used by application.

When we see docker inspect , it gives info about exposed ports

ENV:

ENV is also like a key value pair. It gives info abour environment variables of the image

```
ENV Author="suresh m" \
    Author2="rohan m"
```
we can access these env variables inside the container. It may be helpful to access DB urls or API urls etc

When we do not give any CMD instruction in a docker file, then if give docker run command, it wont run. So we have to give a CMD instruction in docker file, to make something to run in container.

COPY:
Used to copy files from local to image.

The COPY instruction copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>. Files and directories can be copied from the build context, build stage, named context, or an image.

The ADD and COPY instructions are functionally similar, but serve slightly different purposes. Learn more about the differences between ADD and COPY.

ADD:

The ADD instruction copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>. 
Files and directories can be copied from the build context, a remote URL, or a Git repository.

ADD also does the same as COPY, but it has 2 extra capabilities

1. It can get/pulls files from internet URLs
2. It can extract files into image. (ADD sample.tar /tmp/) This command untars the sample.tar in destination location in image. If we use cOPY instruction, it wont extract tar file





