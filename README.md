# Docker

## Helpful links: 
English <br />
https://docs.docker.com/ <br />
http://hua-zhou.github.io/teaching/biostatm280-2019winter/slides/14-docker/docker.html <br />
https://hub.docker.com/search/?q=&type=image <br />
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
Firstly, ensure that you install the appropriate NVIDIA drivers and libraries. You will also need to install ```nvidia-docker2``` to enable GPU device access within Docker containers. This can be found at NVIDIA/nvidia-docker.
 
