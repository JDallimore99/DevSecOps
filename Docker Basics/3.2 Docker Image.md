# Docker Image

## Download the source code
We need to download the source code of the project from our git repository.
```sh
git clone https://gitlab.practical-devsecops.training/pdso/django.nv.git
cd django.nv
```
##Build the image
To build a docker image, we need a file called Dockerfile that contains instructions to build the image.
```sh
cat Dockerfile
**Output**
# FROM python base image
FROM python:2-alpine

# COPY startup script
COPY . /app

WORKDIR /app

RUN apk add --no-cache gawk sed bash grep bc coreutils
RUN pip install -r requirements.txt
RUN chmod +x reset_db.sh && bash reset_db.sh

# EXPOSE port 8000 for communication to/from server
EXPOSE 8000

# CMD specifcies the command to execute container starts running.
CMD ["/app/run_app_docker.sh"]
```
The above Dockerfile has these instructions in it
|Syntax|Description|
|:-----|:----------|
|FROM|Initialize an operating system that will be used as a base|
|COPY|	Copy files or directories from the host to the container|
|WORKDIR|	Sets the working directory similar to cd command on Linux|
|RUN|Execute commands in the current image|
Create the image using the following command
```sh
docker build -t django.nv:1.0 .
**Output**
Sending build context to Docker daemon  7.262MB
Step 1/8 : FROM python:2-alpine
2-alpine: Pulling from library/python
aad63a933944: Pull complete
259d822268fb: Pull complete
10ba96d218d3: Pull complete
44ba9f6a4209: Pull complete
Digest: sha256:724d0540eb56ffaa6dd770aa13c3bc7dfc829dec561d87cb36b2f5b9ff8a760a
Status: Downloaded newer image for python:2-alpine
 ---> 8579e446340f
Step 2/8 : COPY . /app
 ---> 705d799666e6
Step 3/8 : WORKDIR /app
 ---> Running in 895e6b1d0ea9
Removing intermediate container 895e6b1d0ea9
 ---> 103dcfa0202b

...[SNIP]...

  Applying taskManager.0037_auto_20150921_2025... OK
  Applying taskManager.0038_auto_20150921_2027... OK
Installed 48 object(s) from 5 fixture(s)
Removing intermediate container d291597ccccb
 ---> 1422fedc78c7
Step 7/8 : EXPOSE 8000
 ---> Running in 2ac5bd23f0c5
Removing intermediate container 2ac5bd23f0c5
 ---> 41e10b39a6c9
Step 8/8 : CMD ["/app/run_app_docker.sh"]
 ---> Running in de2bf4d387b3
Removing intermediate container de2bf4d387b3
 ---> cba79d46fbce
Successfully built cba79d46fbce
Successfully tagged django.nv:1.0
```
After the build finishes we can see the image using the following command
```sh
docker images
**Output**
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
django.nv           1.0                 cba79d46fbce        10 seconds ago      115MB
python              2-alpine            8579e446340f        5 months ago        71.1MB
```
As you can see above on line number 2, there is a django.nv image with TAG as 1.0.
## Challenge
Rename the docker image django.nv:1.0 to django.nv:1.1
```sh
docker tag django.nv:1.0 django.nv:1.1
```
Run a container named webserver with the image django.nv:1.0 and override entrypoint with bash (/bin/bash) shell
```sh
docker run --name webserver -it --entrypoint /bin/bash django.nv:1.0
```
>Note: You might have to pass -it option to docker run command and exit out of the container before checking the answer.
Delete the docker image django.nv:1.0
```sh
docker rmi django.nv:1.0
```
>Note In case you receive an error message “bash: docker: command not found”, you might need to exit out of the container from the previous step using exit.
