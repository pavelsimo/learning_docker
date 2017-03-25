
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

Commit a container

    docker commit -m "Already install SSH and created test user" -a "pavelsimo" clever_panini pavelsimo/ubuntusshd:v1
    docker run -it pavelsimo/ubuntusshd:v1 /bin/bash

Simple Dockerfile with SSH

    # This is a custom ubuntu image with SSH already installed

    FROM ubuntu:xenial
    MAINTAINER pavelsimo <pavel.simo@gmail.com>
    RUN apt-get update
    RUN apt-get install telnet openssh-server

Building the image

    # if the Dockerfile is in the same directory
    docker build -t="pavelsimo/ubuntusshonly:v2" .

    # if the Dockerfile exist in another directory
    docker build -t="pavelsimo/ubuntusshonly:v2" < /path/to/Dockerfile
 
    # another way of building an image
    docker build -t centos7/nonroot:v1 .

    # running the image
    docker run -it pavelsimo/ubuntusshonly:v2 /bin/bash

See a container logs

    docker logs tender_kalam

Execute container command

    docker exec tender_kalam /bin/cat /etc/profile

Run a command as a daemon:

    docker run -d ubuntu:xenial /bin/bash -c "while true; do echo HELLO; sleep 1; done"

Expose a container port

    # 8000 - HOST PORT
    # 80   - CONTAINER PORT
    docker run -d -p 8000:80 nginx:latest

Remove a Docker Image

    docker rmi e0870be525d9

Connect to container as root

    docker exec -u 0 -it sharp_mayer /bin/bash

Another container example

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com

    RUN useradd -ms /bin/bash user
    RUN echo "EXPORT 192.168.0.0/24" >> /etc/exports.list

    USER user

Creating a Java Container

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com

    RUN useradd -ms /bin/bash user
    RUN echo "EXPORT 192.168.0.0/24" >> /etc/exports.list

    RUN yum update -y
    RUN yum install -y net-tools wget
    RUN cd ~ && wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.rpm"

    RUN yum localinstall -y ~/jre-8u60-linux-x64.rpm

    USER user

    RUN cd ~ && echo "export JAVA_HOME=/usr/java/jdk1.8.0/jre" >> /home/user/.bashrc
    ENV JAVA_BIN /usr/java/jdk1.8.0/jre/bin


