# Docker

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
## Create Docker images with Dockerfile:
For details, see [Dockerfile-reference](https://docs.docker.com/engine/reference/builder/), [Dockerfile-practice](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/). 
```
mkdir ~/my_build
cd ~/my_build
touch Dockerfile
```

### Dockerfile commands:
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/ <br />
https://docs.docker.com/engine/reference/builder/

* An Example of Dockerfile
https://github.com/anibali/docker-pytorch/blob/master/Dockerfile.template

### Build Docker image and run the container:
https://docs.docker.com/engine/reference/builder/
```
$ docker build -t <tag-name> <directory>
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

## Helpful notes:
* Permission: <br />
If your current user can't access the docker engine due to lacking permissions to access the unix socket to communicate with the enigne, run the following command and then completely log out of your account and log back in (or exit your SSH session or reboot the computer).
```
sudo usermod -a -G docker $USER
```
* remove docker image, error response from daemon
```
sudo docker ps -a -q --filter "status=exited" | xargs sudo docker rm
sudo docker rmi `sudo docker images -q --filter "dangling=true"`
```
* Links: <br />
English <br />
https://docs.docker.com/ <br />
http://hua-zhou.github.io/teaching/biostatm280-2019winter/slides/14-docker/docker.html <br />
https://hub.docker.com/search/?q=&type=image <br />
https://www.tensorflow.org/install/docker <br />
Chinese <br />
https://cshihong.github.io/2018/04/02/Docker%E5%9F%BA%E7%A1%80%E5%8E%9F%E7%90%86/ <br />
https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html <br />
