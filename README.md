
Basic Docker commands

    docker info
    docker version
    
    # location where docker images live
    # /var/lib/docker/image
    # /var/lib/docker/image/devicemapper/imagedb/content/sha256

    # location where docker container lives
    # /var/lib/docker/containers

    # pull ubuntu xenial
    docker pull ubuntu:xenial
    docker pull nginx:latest

    # -i interactive mode
    # -t attached to my terminal (tty)
    docker run -i -t ubuntu:xenial /bin/bash

    # restart docker container
    docker restart awesome_sinoussi

    # inspect images
    docker inspect ubuntu:xenial

    # attach to the container
    docker attach awesome_sinoussi

    # -i interactive mode
    # -t attached to my terminal (tty)
    # -d demonized
    docker run -itd ubuntu:xenial /bin/bash

    # obtain container specific information
    docker inspect reverent_thompson

    # start a container
    docker start focused_kilby

    # stop a container
    docker stop reverent_thompson

    # search docker images
    docker search ubuntu
    docker search training/sinatra

    # remove docker image
    docker rmi training/sinatra
    
Build a docker image

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com
    RUN useradd -m -s /bin/bash user
    USER user

    docker build -t centos7/nonroot:v1 .
    docker run -it centos7/noroot:v1 /bin/bash

Connect to a container as root

    docker exec -u 0 -it focused_kilby /bin/bash

Running nginx in docker

    docker pull nginx:latest
    docker run -d nginx:latest

    # This way of running redirect the local port 80 to the container port 80
    docker run -d -p 80:80 nginx:latest
    elinks http://172.17.0.3
    
    # host -> 8080, container -> 80
    docker run -d -p 8080:80 nginx:latest
    elinks http://127.0.0.1:8080
    elinks http://172.17.0.3

Checking container logs

    docker logs sharp_elion

Delete all containers

    docker ps -a | cut -d' ' -f1 | tail -n+2 | xargs docker rm

