# Tools used
## Docker
Docker 
- Self Contained and scalable
- Features and Secuirity
- Simple, Easy and Fast
- Great Support

### Docker Workflow
Create > Store > Pull/Download > Run > Destroy

### Docker Basics
Image - is a tar(zipped) file which explains how a container can be created from the instructions available in image
Container - running an image (run-time) is called container. It virtualises OS using namespaces and cgroups.
Registry - A place where you can store and retrieve Docker images. Docker supports both public and private registries
Repository - Stores a particular image and its various versions in a registry

### Docker Image
Docker Image is like a blueprint/instructions to create a container. container is running form of an image
Images usually created using Dockerfile, which contains step by step instructions on how to create this image.
```sh
$ docker build --tag djangi.nv.
```
### Docker Container
A running image is a container
You can override some part of settings via run command and some you can't
To start a container, you use:
```sh
$ docker run image-name
```
A container can expose ports to outside world for communication
You can do it using the -p/--publish
A container can save the output/result/data using docker volumes. think of them like a shared folder. 
You can share volumes using the -v.--volume option

### Docker Registry
A registry is a place where you can store and retrieve Docker images. Docker supports both public and private registries
Organisaions usually have their private registries on-premises or on cloud. AWS,GCP and Azure has managed registry services.
You need to login to push or pul images from private repos.
A repository is a namespace mechanism to store different versions of an image. Think git repos with multiple releases.
If no image version is given, latest is used by default
To tag an image to a repository, we use -t/--tag
To pull an image from a repository, we use pull command

### Docker Stop & Remove
Once we are done using a container, you can stop it and then rm the container and then image.
You can share volumes using the stop and rm/rmi commands
