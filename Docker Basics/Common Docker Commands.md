|Command|Description|Example|
|:----|:---|:----|
|docker pull|to download the docker image from the docker registry|```docker pull ubuntu```|
|--quiet/-q|hide the output|```docker pull -q alpine```|
|-d|Run a container in the background|```docker run -d --name myubuntu ubuntu```|
|-interactive/-i|Keep the container running|```docker run -d --name myubuntu -i ubuntu```|
|docker rm|remove the previous container|```docker rm myubuntu```|
|docker run|start a container from the docker image.|```docker run -d --name myubuntu ubuntu```|
|docker ps|list|```docker ps```|
|docker exec|execute command inside container or to get the shell in the container|```docker exec myubuntu whoami```|
|-a|See all containers|```docker ps -a```|
|docker top|display running processes inside container|```docker top myubuntu```|
|docker stop|Stop the container|```docker stop webserver && docker rm webserver```|
|-p|Bind container port to all IP addresses on the host|```docker run -d --name=webserver --env APP=nginx -p 80:80 nginx:1.21.3```|
|--name|Names a container|```docker run -d --name webserver --env APP=nginx nginx:1.21.3```|
|-e, --env|Sets the environment variables|```docker run -d --name webserver --env APP=nginx nginx:1.21.3```|
|docker build|Create a docker image|```docker build -t django.nv:1.0 .```|
|docker images|See the docker image|```docker images```|
|dockre tags|Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE|```docker tag django.nv:1.0 django.nv:1.1```
|docker rmi|Remove docker image|```docker rmi django.nv:1.0```|
|docker volume|Manage docker volumes|```docker volume --help```|
|docker volume create|Create a volume|```docker volume create demo```|
| -v /opt:/opt|Bind mounts have been around since the early days of Docker. Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine.|```docker run --name ubuntu2 -d -v /opt:/opt -it ubuntu:18.04```|
