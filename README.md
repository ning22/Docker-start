# Docker

## Helpful links: 
English <br />
https://docs.docker.com/ <br />
http://hua-zhou.github.io/teaching/biostatm280-2019winter/slides/14-docker/docker.html <br />
https://hub.docker.com/search/?q=&type=image <br />
https://www.tensorflow.org/install/docker <br />
Chinese <br />
https://cshihong.github.io/2018/04/02/Docker%E5%9F%BA%E7%A1%80%E5%8E%9F%E7%90%86/ <br />
https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html <br />

## Requirements: 
### Install Docker Engine:
Instructions are available in the Docker website:
https://docs.docker.com/install/linux/docker-ce/ubuntu/ <br />
* Docker Survival commands:
```
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Excecute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -a -q
```

```
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```
```
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```
### CUDA requirements:
Firstly, ensure that you install the appropriate NVIDIA drivers and libraries. You will also need to install ```nvidia-docker2``` to enable GPU device access within Docker containers. This can be found at [NVIDIA/nvidia-docker](https://github.com/NVIDIA/nvidia-docker).

## Prebuilt Docker images:
Pre-built images are available on Docker Hub: <br />
For example, you can download TensorFlow release images to your machine [Tensorflow Docker image](https://www.tensorflow.org/install/docker):
```
$ docker pull tensorflow/tensorflow                     # latest stable release
$ docker pull tensorflow/tensorflow:devel-gpu           # nightly dev release w/ GPU support
$ docker pull tensorflow/tensorflow:latest-gpu-jupyter  # latest release w/ GPU support and Jupyter
```
You can also download the CUDA 10.0 version with pytorch: [Pytorch Docker image](https://hub.docker.com/r/anibali/pytorch/)
```
$ docker pull anibali/pytorch:cuda-10.0
```
## Start a Docker Container:
For details, see [docker-run-reference](https://docs.docker.com/engine/reference/run/).
For example, you can run the following commends
```
$ docker run --runtime=nvidia -it tensorflow/tensorflow:latest-gpu bash
```
```
docker run --rm -it --init \
  --runtime=nvidia \
  --ipc=host \
  --user="$(id -u):$(id -g)" \
  --volume="$PWD:/app" \
  -e NVIDIA_VISIBLE_DEVICES=0 \
  anibali/pytorch python3 main.py
```
## Create Docker images with Dockerfile:
For details, see [Dockerfile-reference](https://docs.docker.com/engine/reference/builder/).
```
mkdir ~/my_build
cd ~/my_build
touch Dockerfile
```
### Dockerfile commands:
* FROM – Select the base image to build the new image on top of
        ``` FROM ubuntu:latest```
* ADD – Defines files to copy from the Host file system onto the Container
``` ADD ./local/config.file /etc/service/config.file
```
    CMD – This is the command that will run when the Container starts
        CMD ["nginx", "-g", "daemon off;"]
    ENTRYPOINT – Sets the default application used every time a Container is created from the Image. If used in conjunction with CMD, you can remove the application and just define the arguments there
        CMD Hello World!
        ENTRYPOINT echo
    ENV – Set/modify the environment variables within Containers created from the Image.
        ENV VERSION 1.0
    EXPOSE – Define which Container ports to expose
        EXPOSE 80

    LABEL maintainer – Optional field to let you identify yourself as the maintainer of this image. This is just a label (it used to be a dedicated Docker directive).
        LABEL maintainer=someone@xyz.xyz"
    RUN – Specify commands to make changes to your Image and subsequently the Containers started from this Image. This includes updating packages, installing software, adding users, creating an initial database, setting up certificates, etc. These are the commands you would run at the command line to install and configure your application. This is one of the most important dockerfile directives.
        RUN apt-get update && apt-get upgrade -y && apt-get install -y nginx && rm -rf /var/lib/apt/lists/*
    USER – Define the default User all commands will be run as within any Container created from your Image. It can be either a UID or username
        USER docker
    VOLUME – Creates a mount point within the Container linking it back to file systems accessible by the Docker Host. New Volumes get populated with the pre-existing contents of the specified location in the image. It is specially relevant to mention is that defining Volumes in a Dockerfile can lead to issues. Volumes should be managed with docker-compose or “docker run” commands. Volumes are optional. If your application does not have any state (and most web applications work like this) then you don’t need to use volumes.
        VOLUME /var/log
    WORKDIR – Define the default working directory for the command defined in the “ENTRYPOINT” or “CMD” instructions
        WORKDIR /home


 
