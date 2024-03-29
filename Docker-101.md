# Docker 101

The motivtion to use __docker__ comes from the difficulty I encounter when I try to set up the enrvironement for Python and install intrigue packages in R. I especially troubled by this when I want to run R script on remote server cluster (HTC at uwmadison). 



I previously just install packages one by one interactively on remote server (ssh). Then set up package path and add it to `.libPaths()`. This is tedious and stressful.



I first learned it through official [tutorial](https://docs.docker.com/get-started/), this tutorial is too general and (too be honest) not good for beginners. It writes Dockerfile not from scratch and biuld an front-end app using js file. I kinda lost in it. Then I found this [introduction to docker](https://carpentries-incubator.github.io/docker-introduction/), this is pretty helpful and concrete and beginner friendly! I know it through a workshop provided by [Datascience Hub](https://datascience.wisc.edu/hub/) at UW-madison. I didn't attend the workshop but turns out to find this fantastic tutorial.

Here are some notes I have, mainly for myself recheck:

__Image__ is like a environment, __container__ is like a slight weight virtual machine.



```bash
Docker container run = docker run

docker container run --help
```



I use busybox as a image example below:

**Run busy box called help page:**

```
docker container run busybox busybox --help    
```



**Run busybox in interactive version:**

```
docker container run -it busybox sh
```



**Remove container once it shut down:**

add `--rm=true` as option

```
docker container run --rm=true -it  busybox sh
```

 

**Run a image container as interactive mode:**

add `-it` as option, and append `bash` or sh in the end

```
docker container run --rm=true -it  busybox sh
```



**Assign a name to container:**

```
docker container run --name=Oliver busybox busybox --help
```



**Remove all containers:**

```
docker container prune
```



**Connected to a stoped container:**

```
docker ps -a         # list all the containers and find your containder

docker start <container ID> # start the exited container 

docker attach <container ID> # attach to your container
```







**Change tag of a docker image:** [**link**](https://stackoverflow.com/questions/25211198/docker-how-to-change-repository-name-or-rename-image)

I use zhuoyanxu/alpine-python as example. 

```
docker tag zhuoyanxu/alpine-python zhuoyanxu/alpine-python:custometag
```



The old `zhuoyanxu/alpine-python` has `latest` as tag. This command create a duplicated image with the same image ID, we can remove the old one by 

```
docker image rm zhuoyanxu/alpine-python     
```

——

### create a container image

**How to create a container image:** [**link**](https://carpentries-incubator.github.io/docker-introduction/creating-container-images/index.html)

Notes about it:

**Interactive installation** 

Interactive installation make create a container with python3. But if start a new container with image alpine, it doesn’t have python3. If we didn’t remove the previous container(where we install python3), we start the container and attach to it, we can find python3.



**Put installation instructions in a Dockerfile**

see link.



——————————

### write Dockerfile to create image

install package from [bioconductor/bioconductor_docker:devel](https://hub.docker.com/r/bioconductor/bioconductor_docker) and create customized image

Explore bioconductor/bioconductor_docker:devel

First open a interactive shell:

```
docker run --rm=true -it bioconductor/bioconductor_docker:devel bash
```

Then find out which packages are available:

```
Rscript -e ".libPaths()"
```

Use `ls -l` on the path to check it



——— create image once we have Dockerfile

make sure dir contains the correct Dockerfile

```
docker build -t bioconductor_ST_zyxu:devel .
```

 

Test it:

```
docker run --user $(id -u):$(id -g) --rm=true -it \

 -v $(pwd):/scratch -w /scratch \

 zhuoyanxu/bioconductor_st_zyxu:devel /bin/bash
```

