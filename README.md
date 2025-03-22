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





