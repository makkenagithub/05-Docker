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

ENV is also like a key value pair. It sets the environment variables in the container. It gives info abour environment variables of the image

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


ENTRYPOINT:

```
FROM almalinux:9
CMD ["ping", "google.com"]
```
build the docker image with above docker file and the run as below
```
docker run -d <image> ping yahoo.com
```
Check the docker logs. It pings the yahoo.com It means CMD can be replaced at run time.

```
FROM almalinux:9
ENTRYPOINT ["ping", "google.com"]
```
build the docker image with above docker file and the run as below
```
docker run -d <image> ping yahoo.com
```
Container will not run in this case. Check the docker logs. It pings like "ping google.com ping yahoo.com" . Its not valid command hence container will not run.
ENTRYPOINT appends ENTRYPOINT command and command given during run time.

We can use ENTRYPOINT and CMD as below for best practices.
```
FROM almalinux:9
#CMD ["ping", "google.com"]
CMD ["google.com"]
ENTRYPOINT ["ping"]
```

```
docker run -d <above-image-name>
```
Now it pings google.com. Hence CMD can be used as to pass arguments to ENTRYPOINT. So the default arguments can be passed through CMD and we can overwrite them at run time.
```
docker run -d <above-image-name> yahoo.com
```
Now CMD will be replaced with yahoo.com and pings yahoo.com, it appends ENTRYPOINT with runtime command yahoo.com

We can use both entrypoint and cmd as above for best practices.

CMD can be used to pass arguments to ENTRYPOINT.


USER:

If we do not provide USER istruction in dockerfile, by default container runs on root user. Hence for security reasons, we should not run container with root.

For security, container must run on normal user. Atleast last instruction in docker file must be USER <user-name> 

If we give USER instruction, then the container will run on that user from the line where the USER instruction used in docker file

WORKDIR:

WORKDIR is used to set the current working directory inside docker image(container). 

cd command does not work in container

ARG:

ARG is used to set the variables at the build time only, not inside the container. ENV is used to set the env variables inside the container and ENV variables can be used during image build time also.

ARG vs ENV:

1. ENV variables can be accessed in image build time and in container both
2. ARG is accessable at the time image creation
3. Usually FROM should be the first instruction in a docker file. But we have an exception, we can use ARG as first instruction to pass version to base image.
4. If we pass ARG as first instruction, then its value is accessable only till FROM instruction. It can be used in FROM instrutcion only, Later its not accessable. 

we can pass the ARG values at the time of build also , we can overwrite default values with build command as below

docker build -t image:v1 --build-arg var1=suresh --build-arg var2=30 .

ONBUILD:

ONBUILD instruction runs only when others use this image. It will not run when we are building this image

Onbuild is used to trigger few instructions at build when a user is using our image.

https://docs.docker.com/reference/dockerfile/#onbuild



DOCKER NETWORK:

When we create an ec2 indtance from AWS, and give command 'ifconfig', we see ens5 wit some private IP (172.31.27.183). So every VM gets access to internet from AWS ISP(internet service provider)

After installing docker in the ec2, give ifconfig command and see. Docker create a virtual network interface with name docker0, and IP(172.17.0.1). This docker0 is called bridge network. This default network of docker.

docker0 acts as modem to the containers inside VM. If we create a container (mysql/backend) , docker0 provides IP to that container. Container IP looks as 172.17.0.2 , we can see container IP wtih docker ps command

Docker containers can't be communicated using default n/w.  When we use docker default network, we can't communicate between containers. For expense project, backend needs to communicate with mqsql. frontend to backend.

TO make the containers to communicate we have to create our own docker network.

TO see list of dcoker networks
```
docker network ls
```
To create a n/w
```
docker netwrok --help
docker network create <network-name- expense>
ifconfig
```
To connect a container to docker network
```
docker network connect <network-name> <container-name/id>
docker network connect expense mysql
```
To disconnect a container from a network / default network
```
docker network ls
docker network disconnect <default-network-name-bridge> <container-name>
```

To place a container to a newtwork while running it
```
docker run -d --network <networkname-expense> --name <container name - backend> backend:v1
docker run -d -p 80:80 --network <n/w-name:expense> --name <container-name- frontend> frontend:v1
```

3 types of networking in Docker networking

1. host network - This is the network provided by ISP, in AWS its AWS ISP. In host network , containers do not get separate IPs for containers.
   Container IP is also host IP. Containers are sharing host IP. Containers open host port. Eg: mqsql container opens 3306 host port, backend opens 8080 host port.

   Below command displays host and bridge networks. We run a mysql container with host network and inspect mysql container. mysql container does not have IP
```
docker network ls
docker run -d --name mysql --network host mysql:v1
docker run -d --name backend --network host backend:v1
docker run -d --name frontend --network host frontend:v1
docker inspect mysql
```
      In host network if one container wants to connect to another eg: backend wants to connect to mqsql, we need to give DB_HOST as localhost. Similarly front to communicate with backedend, we give locahost in config file. 

2. bridge network -  is default bridge network. Give ifconfig command to see it.
      In Bridge network localhost will not work we have to give mysql / backend in config files to connect one container to another. In this default network, dns will not work.
   In bridge network, new IPs are alloted to containers
```
docker network ls
docker network create expense
docker network ls   -> we can see expense network with bridge driver
docker run -d --name mysql --network expense mysql:v1
docker run -d --name backend --network expense backend:v1
docker run -d -p 80:80 --name frontend --network expense frontend:v1
```


3. overlay network - To communicate between multiple docker hosts. To communicate from a container in one server to another container in another server
      But its risky to communicate between multiple hosts containers. Hence we need some orchestration, will come in k8s.
   


DOCKER VOLUMES:

Containers are ephemeral(temporary). They are temporary. Once we remove container, we loose data. Data is not persistent by default. Even if we delete container, data wont be lost if we use volumes.

1. unnamed volumes

We mount the data in local host to containers 

-v host-path:container-path
```
docker run -d -v /home/ec2-user/nginx-date:/usr/share/nginx/html -p 80:80 nginx
```
whatever the data created in container path /usr/share/nginx/html will be stored to server path /home/ec2-user/nginx-date and vice-versa.

All this mounted data stoes in /var/lib/docker directory in server

2. named volumes
   we can manage the named volumes with below docker command. But we can't manage unnamed volumes with docker commands

   ```
   docker volume ls
   docker volume create <volume-name:nginx-html>
   docker volume ls
   docker inspect <voulme-name:nginx-html>
   docker run -d -v <voulme-name:nginx-html>:/usr/share/nginx/html -p 80:80 nginx
   ```
All this mounted data stoes in /var/lib/docker directory in server


DOCKER COMPOSE:

When we have multiple commands in linux, we run those multiple commands by keeping them in a file and we run that file. i.e. shell script.

Here in docker, when we have multiple docker commands to run sequentially like, (building mysql , backend, frontend images and running then with voulmes, networks etc) what we use is called docker compose.

File name is docker-compose.yaml

Docker compose is to up/down multiple serives at a time and we can give volumes, networks, dependencies in docker compose.

command to run docker compose file is
```
docker compose up -d
```
It starts the docker services.

TO stop the docker services and delete the containers
```
docker compose down
```

When we give docker compose , it brings up the containers. If there is any change in docker image and then again if we give docker compose, it checks for changed images and runs them again.

We do not need to remove the old one and run the new one. Docker does that automatically, when there is a change in docker image.

Eg:
```
docker compose up -d
docker build -t joindevops/backend:v1 .
docker compose up -d
```
The above steps delete the old backend image and runs the backed with new image.

Usually the docker insatalled server is to build images. But its not scalable to run container in docker insatlled server. hence we have k8s




DOCKER BEST PRACTICES:

Keep the docker image size small. At max 500MB. Alpine/Distroless image versions have less memory. 

Commands are different for alpine linux OS. Ass user and add gorup commands might be different in alpine linux
```
docker exec it <container-id> sh    --> alpine
docker exec it <container-id> bash    ---> almalinux
```

Layer Caching:

The below command displays information about every command in docker file, when we  build the image. We need to set the env variable and then build the images
```
export DOCKER_BUILDKIT=0
docker build -t joindevops/backend:v1
```
To unset the env variable
```
unset DOCKER_BUILDKIT
```

How docker file runs? :

When we do docker build command, it runs first instruction and creates a container from it and runs 2nd instruction in it and creates 2nd image. 
then it creates container from 2nd image and runs 3rd instruction and so on. We caan see this info after setting the layer env variable DOCKER_BUILDKIT=0

So if we are running a container and made some cahnges in container. We can create  an image from running container with below command
```
docker commit container_id imagename
```
Layer Order: Order your Dockerfile instructions to maximize layer caching. Frequently changing commands should be at the end of the Dockerfile. So we can save build time and memory of layers
Combine Commands: Combine commands where possible to reduce the number of layers, but balance it with readability.

So docker images works based on layering. Every instruction in docker file create an intermediate conatiner and runs next instruction inside it and then saves container as the image layer.
It goes on, at each step intermediate containers are removed and each layer is cached. When we push the image, it pushes the layers


Multi Stage Builds:

It looks like 2 docker files inside a docker file. Refer backend dockerfile for multisatge example.
First file we use it for build and copy its output into 2nd file. It reduces the size of image. For java apps it saves upto 400MB.

Docker ignore:

We need to place .dockerignore file. Other wise all the files placed in the Dockerfile location are loaded by docker. Unnecessary files alose loaded by docker. hence it takes more time to build.

.dockerignore File: Include a .dockerignore file to prevent unnecessary files and directories from being added to the build context, speeding up the build process and keeping the image clean.


We restrict docker usage till image building only. Because docker is not good for scalability for big projects. We use k8s to run images are containers/pods.

Docker architecture:

Docker clinet is nothing but docker command line. 

Docker host/daemon is docker service running

when we give docker run command, docker daemon checks for the image exists locally (local repos) or not. If local image exists it runs it. 
If not, then it pulls from docker hub/registry (remote repos), creates container , runs it, and passes output to clinet.

![image](https://github.com/user-attachments/assets/52f169c4-6fa8-429a-b00d-82b937fc1fe7)

![image](https://github.com/user-attachments/assets/b550a6d3-f66e-49e9-9830-74621f50882b)



