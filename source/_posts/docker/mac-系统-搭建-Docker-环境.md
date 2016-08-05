title: mac 系统 搭建 Docker 环境
date: 2016-02-09 19:48:28
tags:
  - docker
categories:
  - docker
---

## [官方安装](https://docs.docker.com/engine/installation/mac/)





```
# julaud at julauddeMBP.lan in ~ [19:36:51]
$ docker-machine ls
NAME      ACTIVE   URL          STATE     URL                         SWARM   DOCKER   ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.9.1   

# julaud at julauddeMBP.lan in ~ [19:39:41]
$ docker-machine start default
Starting "default"...
Machine "default" is already running.

# julaud at julauddeMBP.lan in ~ [19:40:14]
$ docker-machine ssh default
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.9.1, build master : cef800b - Fri Nov 20 19:33:59 UTC 2015
Docker version 1.9.1, build a34a1d5
docker@default:~$ docker run hello-work
Unable to find image 'hello-work:latest' locally
Pulling repository docker.io/library/hello-work
Error: image library/hello-work:latest not found
docker@default:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b901d36b6f2f: Pull complete 
0a6ba66e537a: Pull complete 
Digest: sha256:8be990ef2aeb16dbcb9271ddfe2610fa6658d13f6dfb8bc72074cc1ca36966a7
Status: Downloaded newer image for hello-world:latest

Hello from Docker.
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/userguide/
```